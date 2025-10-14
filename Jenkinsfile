pipeline{
    agent any 
    stages{
        stage('Code Checkout'){
            steps{
                git branch: 'main', url: 'git@github.com:Roshanx96/demo-terraform.git'
            }
        }
        stage('Terraform Init'){
            steps{
                sh 'terraform init'
            }
        }
        stage('Terraform Plan'){
            steps{
                sh 'terraform plan'
            }
        }
        stage('Terraform Apply'){
            steps{
                sh 'terraform apply -auto-approve'
            }
        }   
    }
    post{
        success{
            echo 'Pipeline executed successfully!'
        }
        failure{
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}