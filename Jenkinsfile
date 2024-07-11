pipeline{
    agent any
    environment {
        AWS_ACCOUNT_ID= credentials('account_id')
        AWS_DEFAULT_REGION="us-east-1" 
        AWS_ACCESS_KEY_ID = credentials('aws_access_key_id')
        AWS_SECRET_ACCESS_KEY = credentials('aws_secret_access_key')
        CHECKOV_VERSION = "2.0.0" // Specify the version of Checkov you want to use
    }
    stages {
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/tkibnyusuf/uber-clone.git'
            }
        }

        stage('Install Checkov') {
            steps {
                script {
                    def checkovInstalled = sh(script: 'checkov --version', returnStatus: true) == 0
                    if (!checkovInstalled) {
                        sh 'pip install checkov==${CHECKOV_VERSION}'
                    }
                }
            }
        }

        stage('Clone Repository') {
            steps {
                git 'https://github.com/tolaoluniyi/uber-clone.git' // Replace with your repository URL
            }
        }

        stage('Run Checkov') {
            steps {
                script {
                    def checkovResult = sh(script: 'checkov -d .', returnStatus: true)
                    if (checkovResult != 0) {
                        currentBuild.result = 'UNSTABLE'
                        echo 'Checkov found issues in the code'
                    } else {
                        echo 'Checkov did not find any issues'
                    }
                }
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
