pipeline {
    agent any

    environment {
        NVD_API_KEY = credentials('nvd-api-key')
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
//                         dependencyCheck additionalArguments: "--scan ./ --out ./ --format ALL --prettyPrint", odcInstallation: "OWASP-DepCheck-10"
                            withCredentials([string(credentialsId: 'nvd-api-key', variable: 'NVD_API_KEY')]) {
                                sh '''
                                   export NVD_API_KEY=$NVD_API_KEY
                                   dependency-check.sh --scan ./ --out ./report --format HTML
                                '''
                            }
                    }
                }
            }
        }
        
    }
}