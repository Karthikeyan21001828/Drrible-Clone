pipeline {
    agent any

    triggers {
        pollSCM('H/1 * * * *') // Polls the SCM every 5 minutes
    }

    // environment {
    //     SONARQUBE = 'SonarQube Test' // Name of the SonarQube server configured in Jenkins
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
        
        // stage('SonarQube Analysis') {
        //     steps {
        //         script {
        //             withSonarQubeEnv(SONARQUBE) {
        //                 sh 'sonar-scanner -Dsonar.projectKey=Html-Sannner -Dsonar.sources=. -Dsonar.host.url=http://192.168.13.135:9000/ -Dsonar.login=sqa_6fdcabf0c9871ee4686ef89e402c9f4485fc50b0'
        //             }
        //         }
        //     }
        // }
        //  stage('SonarQube Quality Gate') {
        //     steps {
        //         script {
        //             // Wait for SonarQube analysis report
        //             timeout(time: 1, unit: 'HOURS') {
        //                 def qg = waitForQualityGate()
        //                 if (qg.status != 'OK') {
        //                     error "Quality gate failed: ${qg.status}"
        //                 }
        //             }
        //         }
        //     }
        //  }
        
        stage('Deploy') {
            steps {
                script {
                    sh '''
                    echo "Deploying application..."
                    sudo rm -rf /var/www/html/*
                    sudo cp -r ${env.WORKSPACE}/* /var/www/html/
                    '''
                    // bat "del /S /Q C:\\DevopsSource\\*"
                    // bat "xcopy /E /I /Y ${env.WORKSPACE} C:\\DevopsSource"
                }
            }
        }
    }
}

