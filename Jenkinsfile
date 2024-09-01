pipeline {
    agent any

    triggers {
        pollSCM('H/1 * * * *') // Polls the SCM every minute
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

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(SONARQUBE) {
                        sh '''
                        #!/bin/bash
                        sonar-scanner \
                            -Dsonar.projectKey=Drrible-Clone \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://192.168.13.135:9000 \
                            -Dsonar.login=${sqa_943f04b21282cbb5473643810b908501f4a4c48b}
                        '''
                    }
                }
            }
        }
        
        stage('SonarQube Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Quality gate failed: ${qg.status}"
                        }
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Use bash to avoid bad substitution errors
                    sh '''#!/bin/bash
                    echo "Deploying application..."
                    sudo rm -rf /var/www/html/*
                    sudo cp -r ${WORKSPACE}/* /var/www/html/
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}
