pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/Karthikeyan21001828/Drrible-Clone.git', 
                    credentialsId: 'github-pat'
            }
        }
        stage('Deploy to IIS') {
            steps {
                script {
                    def targetDir = 'D:\Devops\Drrible-Clone'
                    bat """
                        echo Deploying website to IIS
                        xcopy /s /e /y . ${targetDir}
                    """
                }
            }
        }
    }
}
