pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/Karthikeyan21001828/Drrible-Clone.git', 
                    credentialsId: 'github-pat'
            }
        }
        stage('Build') {
            steps {
                echo 'No build necessary for static content'
            }
        }
        stage('Deploy to IIS') {
            steps {
                script {
                    def targetDir = 'C:\\inetpub\\wwwroot\\DribbbleClone'
                    bat """
                        echo Deploying website to IIS
                        xcopy /s /e /y . ${targetDir}
                    """
                }
            }
        }
    }
}
