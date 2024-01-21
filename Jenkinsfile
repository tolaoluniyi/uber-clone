pipeline{
    agent any
    environment {
        AWS_ACCOUNT_ID="775012328020"
        AWS_DEFAULT_REGION="us-east-1"    
    stages {
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/SushantOps/uber-clone.git'
            }
        }
        stage('Terraform version'){
             steps{
                 sh 'terraform --version'
             }
        }
        stage('Terraform init'){
           environment {
             AWS_ACCESS_KEY_ID = credentials('aws_access_key_id')
             AWS_SECRET_ACCESS_KEY = credentials('aws_secret_access_key')
           }            
           steps{
                dir('EKS_TERRAFORM') {
                      sh 'terraform init'
                   }
             }
        }
        stage('Terraform validate'){
             steps{
                 dir('EKS_TERRAFORM') {
                      sh 'terraform validate'
                   }
             }
        }
        stage('Terraform plan'){
             steps{
                 dir('EKS_TERRAFORM') {
                      sh 'terraform plan'
                   }
             }
        }
        stage('Terraform apply/destroy'){
             steps{
                 dir('EKS_TERRAFORM') {
                      sh 'terraform ${action} --auto-approve'
                   }
             }
        }
    }
}
}    
