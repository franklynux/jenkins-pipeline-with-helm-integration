pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        CLUSTER_NAME = 'my-capstone-cluster'
        KUBECONFIG = "$HOME/.kube/config"
        CHART_DIR = "charts/your-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/franklynux/jenkins-pipeline-with-helm-integration.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("your-dockerhub-username/your-webapp:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to EKS with Helm') {
            steps {
                sh '''
                aws eks update-kubeconfig --region $AWS_DEFAULT_REGION --name $CLUSTER_NAME
                helm upgrade --install your-webapp-release $CHART_DIR --set image.repository=your-dockerhub-username/your-webapp,image.tag=${BUILD_NUMBER}
                '''
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
