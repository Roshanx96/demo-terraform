pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
    }

    stages {
        stage('Code Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Roshanx96/demo-terraform.git'
            }
        }

        stage('Login to AWS') {
            steps {
                script {
                    withAWS(credentials: 'aws-cred', region: "${AWS_REGION}") {
                        echo '✅ Logged into AWS successfully!'
                        sh 'aws sts get-caller-identity'
                    }
                }
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    withAWS(credentials: 'aws-cred', region: "${AWS_REGION}") {
                        sh 'terraform init'
                    }
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    withAWS(credentials: 'aws-cred', region: "${AWS_REGION}") {
                        sh 'terraform plan'
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    withAWS(credentials: 'aws-cred', region: "${AWS_REGION}") {
                        sh 'terraform apply -auto-approve'
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Please check the logs.'
        }
    }
}
