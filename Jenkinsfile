pipeline {
	agent any
	
	options {
		skipStagesAfterUnstable()
		
		timeout(time:1, unit:'HOURS')
	}
	
	environment {
		def USERMAIL = "1058180192@qq.com;lei.fan@capgemini.com"
		MAVEN_OPTS="-Xmx125m"
	}
	
	stages {
		
		stage('Info') {
			steps {
				sh 'git branch'
                echo 'BRANCH_NAME'
				sh 'printenv'
			}
			
		}
		stage('Build') {
			steps {
				echo 'Starting building'
				sh 'mvn -B -DskipTests clean package'
			}
		}
		
		stage ('Test') {
			steps {
				sh mvn sonar:sonar -Dsonar.projectKey=caas -Dsonar.host.url=http://sonar-sonarqube.devops:9000 -Dsonar.login=18525d48f80feb61692f63d0e378682e4a34c915
			}
			
			post {
				always {
					archiveArtifacts 'target/*.jar'
				}
			}
			
		}
		
		stage('Deploy for development') {
			when {
				branch 'dev'
			}
			
			input {
				message 'Deploy for deployment?'
				ok 'yes'
			}
			
			steps {
				echo 'This deploy process is for development'
			}
			
		}
		
		stage('Deploy for production') {
			when {
				branch 'master'
			}
			
			input {
				message 'Deploy for production？'
				ok 'yes'
			}
			 
			steps {
				echo 'This process is for production'
			}
		}
	}
	
	post {
		always {
			echo "The result of this pipeline has sent to your mailbox"
            //删除当前目录
			deleteDir()
		}
		
		success {
			script{
				configFileProvider([configFile(fileId: 'caas-email-template-20191216',
											   targetLocation: 'email.html', 
											   variable: 'failt_email_template')]) {
					template = readFile encoding: 'UTF-8', file: "${failt_email_template}"
					emailext(subject: '任务执行成功',
							 attachLog: true,
							 recipientProviders: [requestor()], 
							 to: "${USERMAIL}",
							 body: """${template}""")
					}
				}

		}
		
		failure {
			script{
				configFileProvider([configFile(fileId: 'caas-email-template-20191216',
											   targetLocation: 'email.html', 
											   variable: 'failt_email_template')]) {
					template = readFile encoding: 'UTF-8', file: "${failt_email_template}"
					emailext(subject: '任务执行失败',
							 attachLog: true,
							 recipientProviders: [requestor()], 
							 to: "${USERMAIL}",
							 body: """${template}""")
					}
				}

		}
		
	}
}
