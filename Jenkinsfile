pipeline {
    agent any
    environment {
        SONAR_TOKEN = "squ_1fd03820e36faf6d4620eea0f90c2e7d23c024e4" // Use the stored Jenkins credential
    }
    // triggers {
    //     pollSCM('H/1 * * * *') // Polls the SCM every minute
    // }

    stages {
        
        stage('Set PATH') {
            steps {
                script {
                    // Prepend SonarQube Scanner path
                    env.PATH = "/opt/sonar-scanner/bin:${env.PATH}"
                }
            }
        }
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
                        
                        sh '''
                        #!/bin/bash
                        sonar-scanner \
                            -Dsonar.projectKey=my_project \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://44.222.70.89:9000 \
                            -Dsonar.login=${SONAR_TOKEN} \
                            -Dsonar.language=html,css \
                            -Dsonar.ws.timeout=300000 
                        '''
                }
            }
        }
        
        stage('SonarQube Quality Gate') {
            steps {
                script {
                    def projectKey = 'my_project'
                    def sonarQubeUrl = 'http://44.222.70.89:9000'
                    def sonarUser = 'admin'
                    def sonarPassword = 'admin123'
        
                    // Fetch the API response
                    def response = sh(script: """
                        curl -u ${sonarUser}:${sonarPassword} "${sonarQubeUrl}/api/qualitygates/project_status?projectKey=${projectKey}"
                    """, returnStdout: true).trim()
        
                    // Log the raw API response for debugging
                    echo "Raw API Response: ${response}"
        
                    // Parse JSON response
                    def jsonResponse = readJSON text: response
                    def qualityGateStatus = jsonResponse.projectStatus.status
        
                    // Log the quality gate status
                    echo "Quality Gate Status: ${qualityGateStatus}"
        
                    // Check the quality gate status
                    if (qualityGateStatus == 'NONE') {
                        error "Quality gate status is 'NONE'. Please check SonarQube configuration."
                    } else if (qualityGateStatus != 'OK') {
                        error "Quality gate failed: ${qualityGateStatus}"
                    } else {
                        echo "Quality gate passed successfully."
                    }
                }
            }
        }

        
        stage('Deploy') {
            steps {
                script {
                   def apacheContainerName = 'apache'

                    // Define the target directory in the Apache container
                    def targetDirectory = '/var/www/html/'

                    // Use 'docker cp' to copy files from the Jenkins workspace to the Apache container
                    sh """
                    echo "Deploying application to Apache container..."
                    docker cp ${WORKSPACE}/. ${apacheContainerName}:${targetDirectory}
                    """
                }
            }
        }
    }
}
