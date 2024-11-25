pipeline {
    agent {
        label 'AGENT-1'
    }

    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        //retry(1)
    }

    environment {
        DEBUG = 'true'
        appVersion = '' // this is global env we can use across pipeline
    }

    stages {
        stage('Read the version') {
            steps {
               scripts { 
                    def packageJson = readJson file: 'package.json'
                    appVersion = packageJson.version
                    echo "App version is: ${appVersion}"
               }
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Docker build') {
            steps {
                sh """
                    docker build -t skkumari6669/backend:${appVersion} .
                    docker images
                """
            }
        }

        stage('print params') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"
            }
        }

        stage('Example') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }

    }

    post { 
        always {
            echo 'this section runs always'
        }

        success {
            echo 'this section runs when pipe line success'
        }

        failure {
            echo 'this section runs when pipeline is failure'
        }
    }
}