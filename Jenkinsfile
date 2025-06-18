pipeline {
    agent any

    environment {
        NVD_API_KEY = credentials('2a7f503a-2573-40dd-ba13-a30d2b6bec79')
    }

    tools {
        nodejs "nodejs-22-6-0"
    }
    stages {
        stage("Installing Dependencies") {
            steps {
                sh "npm i --no-audit"
            }
        }
        stage("Dependency Scanning") {
            parallel {
                stage("NPM Dependency Audit") {
                    steps {
                        sh '''
                            npm audit --audit-level=critical
                            echo $?
                        '''
                    }
                }
                stage("OWASP Dependency Check") {
                    steps {
                        dependencyCheck additionalArguments: "--scan ./ --out ./ --format ALL --prettyPrint", odcInstallation: "OWASP-DepCheck-10"

                    }
                }
            }
        }
        
    }
}