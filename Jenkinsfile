pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        CLUSTER_NAME = 'helm-app-cluster'
        WORKSPACE = "/var/lib/jenkins/workspace/${JOB_NAME}"
        KUBECONFIG = "${env.WORKSPACE}/.kube/config"
        CHART_DIR = "devopsdemo"
    }

    stages {
        stage('Debug Environment') {
            steps {
                sh '''
                echo "AWS_DEFAULT_REGION: $AWS_DEFAULT_REGION"
                echo "CLUSTER_NAME: $CLUSTER_NAME"
                echo "WORKSPACE: $WORKSPACE"
                echo "KUBECONFIG: $KUBECONFIG"
                echo "CHART_DIR: $CHART_DIR"
                '''
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/franklynux/jenkins-pipeline-with-helm-integration.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("franklynux/devopsdemo:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Verify AWS Credentials') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials']]) {
                    sh 'aws sts get-caller-identity'
                }
            }
        }

        stage('Prepare Kubeconfig Directory') {
            steps {
                sh 'mkdir -p ${WORKSPACE}/.kube'
            }
        }

        stage('Deploy to EKS with Helm') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials']]) {
                    sh """
                    aws eks update-kubeconfig --region $AWS_DEFAULT_REGION --name $CLUSTER_NAME
                    helm upgrade --install devopsdemo $CHART_DIR --set image.repository=franklynux/devopsdemo,image.tag=${BUILD_NUMBER}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}