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
        stage('Info') {
            echo 'This stage is used for showing some basic information'
            sh 'mvn --version'
            sh 'java -version'
        }
        stage('Build') { 
            steps {
                echo 'Build Step which will package our project without any test'
                sh 'mvn -B -DskipTests clean package'
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
            echo 'This will find the xml file generated by test peocess'
            junit 'target/surefire-reports/*.xml'
        }
    }
}
