pipeline {
    agent any

    environment {
        DEPLOY_SERVER = '192.168.36.16'
    }
     // triggers {
     //     pollSCM('H/2 * * * *') // Polls every 5 minutes
     // }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/Karthikeyan21001828/Drrible-Clone.git']]
                ])
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Remove the existing files
                    bat "del /S /Q ${'C:\\inetpub\\Devops'}\\*"
                    
                    // Copy the new files from the repository to the deployment folder
                    bat "xcopy /E /I /Y ${env.WORKSPACE} ${'C:\\inetpub\\Devops'}"
                }
            }
        }
    }
}

