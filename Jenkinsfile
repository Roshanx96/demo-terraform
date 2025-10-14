pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
    }

    parameters {
        choice(
            name: 'ACTION',
            choices: ['apply', 'destroy'],
            description: 'Choose Terraform action to perform'
        )
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

        stage('Terraform Apply or Destroy') {
            steps {
                script {
                    withAWS(credentials: 'aws-cred', region: "${AWS_REGION}") {
                        if (params.ACTION == 'apply') {
                            echo '🚀 Running Terraform Apply...'
                            sh 'terraform apply -auto-approve'
                        } else if (params.ACTION == 'destroy') {
                            echo '🔥 Running Terraform Destroy...'
                            sh 'terraform destroy -auto-approve'
                        } else {
                            error("❌ Invalid action: ${params.ACTION}")
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Terraform ${params.ACTION} executed successfully!"
        }
        failure {
            echo "❌ Terraform ${params.ACTION} failed. Please check the logs."
        }
    }
}
