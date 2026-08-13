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

                    withSonarQubeEnv('Sonar') {
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
        stage("Quality Gate") {
            steps {
                waitForQualityGate abortPipeline: false, credentialsId: 'Sonar'
            }
        }
        stage("Run Unit Tests") {
            steps {
                sh 'go test ./...'
            }
        }
        stage("Build") {
            steps {
                sh """
                   printenv
                   docker build -t ${IMAGE_NAME} .
                   """
            }
        }
        stage("Scan") {
            steps {
                sh """ 
                   trivy image ${IMAGE_NAME} >> frontend-report.txt
                   """
                   }
        }
        stage {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'Docker') {
                        sh """
                           docker push ${IMAGE_NAME}
                           """
                    }
                }
            }
        }
    }
}
