pipeline {
    agent any
    tools {
        nodejs '20.0.0'
    }
   
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'sh "pip3 install -r requirements.txt"'
            }
        }
      
    }
}
