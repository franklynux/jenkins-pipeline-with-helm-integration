pipeline {
    agent any

    environment {
        // Define global environment variables for the pipeline execution
        AWS_DEFAULT_REGION = 'us-east-1'            // AWS region where EKS is deployed
        CLUSTER_NAME = 'helm-app-cluster'           // Name of the EKS cluster
        WORKSPACE = "/var/lib/jenkins/workspace/${JOB_NAME}"  // Custom workspace path for the job
        KUBECONFIG = "${env.WORKSPACE}/.kube/config"          // Path to kubeconfig for authenticating with the cluster
        CHART_DIR = "devopsdemo"                    // Path to the Helm chart directory inside the repo
    }

    stages {
        stage('Debug Environment') {
            steps {
                // Print environment variables to the console for validation and debugging
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
                // Clone the application's Git repository (main branch) into Jenkins workspace
                git branch: 'main', url: 'https://github.com/franklynux/jenkins-pipeline-with-helm-integration.git'
            }
        }

        stage('Verify AWS Credentials') {
            steps {
                // Use Jenkins stored AWS credentials (IAM user/role) to verify that AWS CLI is configured properly
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials']]) {
                    // Display AWS caller identity for audit and debugging
                    sh 'aws sts get-caller-identity'
                }
            }
        }

        stage('Prepare Kubeconfig Directory') {
            steps {
                // Ensure the .kube directory exists inside the Jenkins workspace for storing the kubeconfig
                sh 'mkdir -p ${WORKSPACE}/.kube'
            }
        }

        stage('Deploy to EKS with Helm') {
            steps {
                // Use AWS credentials to:
                // 1. Update kubeconfig to authenticate and connect to the EKS cluster.
                // 2. Perform Helm upgrade (or install if not existing) of the 'devopsdemo' release.
                // 3. Set the image repository and dynamically set the image tag to Jenkins build number.
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
        // Post-execution steps that notify about the pipeline result
        success {
            // Message when deployment succeeds
            echo 'Deployment successful!'
        }
        failure {
            // Message when deployment fails
            echo 'Deployment failed.'
        }
    }
}
