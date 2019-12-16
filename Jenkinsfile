pipeline {
	agent any
	
	options {
		skipStagesAfterUnstable()
		
		timeout(time:1, unit:'HOURS')
	}
	
	environment {
		def USERMAIL = "1058180192@qq.com;lei.fan@capgemini.com"
		MAVEN_OPTS = "-Xmx512M"
		//-XX:MaxPermSize=512M
	}
	
	stages {

		stage('Build') {
			steps {
				echo 'Starting building'
				//deleteDir()
				sh 'mvn -B -DskipTests clean package'
			}
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
			//input {
				//message "Do you want to download the file?"
				//ok "yes"
			//}
			
			archiveArtifacts 'target/*.jar'
		}
		
		success {
		    emailext (
			subject: "'${env.JOB_NAME} [${env.BUILD_NUMBER}]' 更新正常",
			body: """
			详情：
			SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'
			状态：${env.JOB_NAME} jenkins 更新运行正常 
			URL ：${env.BUILD_URL}
			项目名称 ：${env.JOB_NAME} 
			项目更新进度：${env.BUILD_NUMBER}
			""",
			to: "${USERMAIL}",  
			recipientProviders: [[$class: 'DevelopersRecipientProvider']]
			)
                }
		
	}
}
