pipeline {
    agent any
    tools {
        nodejs '20.0.0'
    }

    stages {
        stage('Install Python Dependencies') {
            steps {
                withPythonEnv('python3') {
                    sh '/Users/mprzybus/.jenkins/workspace/validate-api-doc/.pyenv-python3/bin/python3 -m pip install --upgrade pip'
                    sh 'pip3 install -r requirements.txt'
                }
            }
        }
        stage('Install Npm Dependencies') {
            steps {
                sh 'npm install -g @redocly/cli'
                sh 'npm install -g swagger-cli'
                sh 'npm install -g rdme'
                sh 'npm install -g openapi-to-postmanv2'
                sh 'npm install -g openapi-enforcer-cli'
            }
        }
        stage('Checkout Validator ') {
            steps {
                dir('my_app') {

                    sh "cp -r /Users/mprzybus/Desktop/akamai-readmeio-tooling-SOAP/swag-tools/src/* ."
                }
            }
        }
        stage('Run Validation') {
            steps {
                dir('reference-api/v1') {
                    withPythonEnv('python3') {

                        sh 'python3 /Users/mprzybus/.jenkins/workspace/validate-api-doc/my_app/swag-tool preview'

                    }

                }
            }
        }

        stage('Check Blocking Errors') {
            steps {
                dir('reference-api/v1') {
                    script {
                        def blockingContent = readFile 'blocking.txt'
                        if (blockingContent) {
                            error 'Build failed: Address blocking issues'
                        }
                    }
                }
            }
        }

    }

}