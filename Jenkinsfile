pipeline {
    agent {
        node {
            label 'master-node'
        }
    }
    
    tools {
        maven 'Maven-3.6.3'
    }
    
    stages {
        stage('Build') { 
            steps {
                echo 'This is a minimal pipeline'
                sh 'mvn --version'
                sh 'java -version'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
    
    post {
        always {
            echo 'This will always be shown'
        }
    }
}
