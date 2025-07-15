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
//                 stage("OWASP Dependency Check") {
//                     steps {
//                         dependencyCheck additionalArguments: '--nvdApiKey $NVD_API_KEY --scan ./ --out ./ --format ALL --prettyPrint', odcInstallation: "OWASP-DepCheck-10"
//
//                         publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'dependency-check-jenkins.html', reportName: 'Dependency Check HTML Report', reportTitles: '', useWrapperFileDirectly: true])
//                     }
//                 }
            }
        }

        stage("Unit Testing") {
            steps {
                sh "npm test"
            }
        }
        
    }
}