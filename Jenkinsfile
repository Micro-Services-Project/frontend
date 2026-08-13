pipeline {
    agent any
    environment {
        image-name= "obitomanu/frontend:${GIT_COMMIT}"
    }
    stages {
        stage("clean workspace") {
            steps {
                cleanWs()
            }
        }
    }
}