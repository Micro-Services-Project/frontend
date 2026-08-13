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
        stage("Run Unit Tests") {
            steps {
                sh 'go test ./...'
            }
        }
    }
}