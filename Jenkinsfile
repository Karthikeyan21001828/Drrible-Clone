pipeline {
    agent any
    environment {
        TERRAFORM_PATH = 'C:\\Users\\Karthikeyan\\Terraform'
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
                    dir("${env.TERRAFORM_PATH}") {
                        bat "terraform init"
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    dir("${env.TERRAFORM_PATH}") {
                        bat "terraform apply -auto-approve"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    bat "del /S /Q C:\\inetpub\\Devops\\*"
                    bat "xcopy /E /I /Y ${env.WORKSPACE} C:\\inetpub\\Devops"
                }
            }
        }
    }
}
