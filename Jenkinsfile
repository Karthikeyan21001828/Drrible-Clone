pipeline {
    agent any
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
                    bat "del /S /Q C:\\DevopsSource\\*"
                    bat "xcopy /E /I /Y ${env.WORKSPACE} C:\\DevopsSource"
                }
            }
        }
    }
}
