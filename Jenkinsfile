pipeline{
    agent any
    environment {
        AWS_ACCOUNT_ID= credentials('account_id')
        AWS_DEFAULT_REGION="us-east-1" 
        AWS_ACCESS_KEY_ID = credentials('aws_access_key_id')
        AWS_SECRET_ACCESS_KEY = credentials('aws_secret_access_key')
    }
    stages {
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/tkibnyusuf/uber-clone.git'
            }
        }

        Stage('Checkov Code Scan') {
               script {
                    //checkov Steps
                    //1. Install Checkov
                    //2. Run the Checkov Scan
                    sh """
                        pip install checkov==${env.CHECKOV_VERSION}
                    """
                    sh """
                        checkov -d .
                    """
                }
        }
        stage('Terraform version'){
             steps{
                 sh 'terraform --version'
             }
        }
        stage('Terraform init'){           
           steps{
                dir('EKS_Terraform') {
                      sh 'terraform init'
                   }
             }
        }
        stage('Terraform validate'){
             steps{
                 dir('EKS_Terraform') {
                      sh 'terraform validate'
                   }
             }
        }
        stage('Terraform plan'){
             steps{
                 dir('EKS_Terraform') {
                      sh 'terraform plan'
                   }
             }
        }
        stage('Terraform apply/destroy'){
             steps{
                 dir('EKS_Terraform') {
                      sh 'terraform ${action} --auto-approve'
                   }
             }
        }
    }
}  
