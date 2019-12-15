pipeline {
	agent any
	
	options {
		skipStagesAfterUnstable()
		
		timeout(time:1, unit:'HOURS')
	}
	
	
	stages {

		stage('Build') {
			echo 'Starting building'
			deleteDir()
			sh 'mvn -B -DskipTests clean package'
		}
		
		stage ('Test') {
			steps {
				sh 'mvn test'
			}
			
			post {
				always {
					echo 'The result has sent to you mailbox'
					junit 'target/surefire-reports/*.xml'
				}
			}
			
		}
		
		stage('Deploy for development') {
			when {
				beforeInput true
				
				branch 'dev'
			}
			
			input {
				message 'Being sure to continue?'
				ok 'yes'
			}
			
			steps {
				echo 'This deploy process is for development'
			}
			
		}
		
		stage('Deploy for production') {
			when {
				beforeInput true
				
				branch 'master'
			}
			
			input {
				message 'Being sure to continue?'
				ok 'yes'
			}
			 
			steps {
				echo 'This process is for deployment'
			}
		}
	}
	
	post {
		always {
			
			input {
				message 'Do you want to download the file?'
				ok 'yes' 
			}
			
			archiveArtifacts 'target/*.jar'
		}
		
	}
}
