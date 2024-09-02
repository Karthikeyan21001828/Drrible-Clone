pipeline {
    agent any

    // triggers {
    //     pollSCM('H/1 * * * *') // Polls the SCM every minute
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

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') { // Use the name of your SonarQube server here
                        sh '''
                        #!/bin/bash
                        /opt/sonarscanner/bin/sonar-scanner \
                            -Dsonar.projectKey=DevOpsTest \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://16.171.225.60:9000 \
                            -Dsonar.token=sqp_f0fb50f30a450f1a0e4ff750f2cfba7462c936dc \
                            -Dsonar.language=html,css \
                            -Dsonar.ws.timeout=300000 
                           
                        '''
                    }
                }
            }
        }
        
        stage('SonarQube Quality Gate') {
            steps {
                script {
                    def projectKey = 'DevOpsTest'
                    def sonarQubeUrl = 'http://16.171.225.60:9000'
                    def sonarUser = 'admin'
                    def sonarPassword = 'redhat'

                    def response = sh(script: """
                        curl -u ${sonarUser}:${sonarPassword} "${sonarQubeUrl}/api/qualitygates/project_status?projectKey=${projectKey}"
                    """, returnStdout: true).trim()

                    def jsonResponse = readJSON text: response
                    def qualityGateStatus = jsonResponse.projectStatus.status

                    echo "Quality Gate Status: ${qualityGateStatus}"

                    if (qualityGateStatus != 'OK') {
                        error "Quality gate failed: ${qualityGateStatus}"
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
