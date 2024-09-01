pipeline {
    agent any

    triggers {
        pollSCM('H/1 * * * *') // Polls the SCM every 1 minute
    }

    environment {
        // Uncomment and configure if using SonarQube
        // SONARQUBE = 'SonarQube Test' // Name of the SonarQube server configured in Jenkins
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
        
        // Uncomment and configure if using SonarQube
        // stage('SonarQube Analysis') {
        //     steps {
        //         script {
        //             withSonarQubeEnv(SONARQUBE) {
        //                 sh 'sonar-scanner -Dsonar.projectKey=Html-Sanner -Dsonar.sources=. -Dsonar.host.url=http://192.168.13.135:9000/ -Dsonar.login=sqa_6fdcabf0c9871ee4686ef89e402c9f4485fc50b0'
        //             }
        //         }
        //     }
        // }
        // stage('SonarQube Quality Gate') {
        //     steps {
        //         script {
        //             timeout(time: 1, unit: 'HOURS') {
        //                 def qg = waitForQualityGate()
        //                 if (qg.status != 'OK') {
        //                     error "Quality gate failed: ${qg.status}"
        //                 }
        //             }
        //         }
        //     }
        // }

        stage('Deploy') {
            steps {
                script {
                    // Use bash to avoid bad substitution errors
                    sh '''#!/bin/bash
                    echo "Deploying application..."
                    sudo rm -rf /var/www/html/*
                    sudo cp -r ${env.WORKSPACE}/* /var/www/html/
                    '''
                }
            }
        }
    }
    
    post {
        always {
            // Clean up workspace if needed
            cleanWs()
        }
    }
}
