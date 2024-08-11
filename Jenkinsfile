pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Karthikeyan21001828/https://github.com/Karthikeyan21001828/Drrible-Clone.git'
            }
        }
        stage('Build') {
            steps {
                // No build step needed for a static website, but you can add any pre-processing here
                echo 'No build necessary for static content'
            }
        }
        stage('Deploy to IIS') {
            steps {
                script {
                    def targetDir = 'C:\\inetpub\\wwwroot'  // Target directory for IIS
                    bat """
                        echo Deploying website to IIS
                        xcopy /s /e /y . ${targetDir}  // Copy files to IIS root directory
                    """
                }
            }
        }
    }
}
