pipeline {
    agent any

    environment {
        TERRAFORM_PATH = 'C:/Terraform/terraform.exe'
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
                    dir("${env.TERRAFORM_DIR}") {
                        bat "${env.TERRAFORM_PATH} init"
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    dir("${env.TERRAFORM_DIR}") {
                        bat "${env.TERRAFORM_PATH} apply -auto-approve"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    bat "del /S /Q C:\\DevopsSource\\*"
                    bat "xcopy /E /I /Y ${env.WORKSPACE} C:\\DevopsSource"
                }
            }
        }
    }
}
