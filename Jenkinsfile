pipeline {
    agent any
    environment {
        IMAGE_NAME="obitomanu/frontend:${GIT_COMMIT}"
    }
    stages {
        stage("clean workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Git-checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/Micro-Services-Project/frontend.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'Sonar'

                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=frontend \
                            -Dsonar.projectName=frontend \
                            -Dsonar.sources=.
                        """
                    }
                }
            }
        }
        stage("Run Unit Tests") {
            steps {
                sh 'go test ./...'
            }
        }
    }
}