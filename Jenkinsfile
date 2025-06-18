pipeline {
    agent any

    tools {
        nodejs "nodejs-22-6-0"
    }
    stages {
        stage("Node Version") {
            steps {
                sh '''
                    node -v
                    npm -v
                '''
            }
        }
    }
}