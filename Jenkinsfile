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
    }
}