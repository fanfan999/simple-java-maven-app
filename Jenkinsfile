pipeline {
    agent {
        docker {
            image 'maven:latest'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') { 
            steps {
                echo 'This is a minimal pipeline'
                sh 'mvn --version'
            }
        }
    }
}
