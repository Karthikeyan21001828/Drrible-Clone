pipeline {
    agent any
     environment {
        TERRAFORM_PATH = 'C:/Terraform' 
        TERRAFORM_DIR = 'C:/Users/Karthikeyan/Terraform'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/Karthikeyan21001828/Drrible-Clone.git']]
                ])
            }
        }
        stage('Terraform Init') {
            steps {
                script {
                    // Navigate to the Terraform directory and initialize Terraform
                    dir("${env.TERRAFORM_DIR}") {
                        sh "${env.TERRAFORM_PATH} init"
                    }
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    // Run Terraform plan in the Terraform directory
                    dir("${env.TERRAFORM_DIR}") {
                        sh "${env.TERRAFORM_PATH} plan"
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    // Run Terraform apply in the Terraform directory
                    dir("${env.TERRAFORM_DIR}") {
                        sh "${env.TERRAFORM_PATH} apply -auto-approve"
                    }
                }
            }
        }
    }
        stage('Deploy') {
            steps {
                script {
                    bat "del /S /Q ${'C:\\inetpub\\Devops'}\\*"
                    
                    bat "xcopy /E /I /Y ${env.WORKSPACE} ${'C:\\inetpub\\Devops'}"
                }
            }
        }
    }
}

