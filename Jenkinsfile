pipeline {
    agent any
    environment {
       PYENCHANT_LIBRARY_PATH = "/opt/homebrew/lib/libenchant-2.2.dylib"                               //can be used in whole pipeline
    } 
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
        stage('Identify Parent Folder') {
            steps {
                script {
                    def changedFiles = sh (
                        returnStdout: true,
                        script: 'git diff --name-only HEAD~1..HEAD'
                    ).trim().split('\n')

                    def nearestParentFolder = null

                    for (file in changedFiles) {
                        def parentFolder = file.substring(0, file.lastIndexOf('/'))
                        def match = parentFolder =~ /v[1-9]$/
                        if (match) {
                            nearestParentFolder = parentFolder
                            break
                        }
                    }

                    if (nearestParentFolder) {
                        echo "Nearest parent folder: ${nearestParentFolder}"
                        env.PARENT_FOLDER = nearestParentFolder
                    } else {
                        echo "No changes found in a parent folder matching the pattern v[1-9]."
                    }
                }
            }
        }
        stage('Run Validation') {
            steps {
                dir("${env.PARENT_FOLDER}") {
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
