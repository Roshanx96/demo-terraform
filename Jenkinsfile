pipeline{
    agent any 
    stages{
        stage('Code Checkout'){
            steps{
                git branch: 'main', url: 'git@github.com:Roshanx96/demo-terraform.git'
            }
        }
        stage('AWS Login'){
            steps{
                // Load AWS credentials once and export them to the pipeline environment
                withCredentials([usernamePassword(credentialsId: 'aws-creds', usernameVariable: 'TMP_AWS_ACCESS_KEY_ID', passwordVariable: 'TMP_AWS_SECRET_ACCESS_KEY')]){
                    script {
                        // set them in the pipeline env so later stages can use them
                        env.AWS_ACCESS_KEY_ID = TMP_AWS_ACCESS_KEY_ID
                        env.AWS_SECRET_ACCESS_KEY = TMP_AWS_SECRET_ACCESS_KEY
                    }
                }
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