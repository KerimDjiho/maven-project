pipeline {
	/* A declarative Pipeline */
	agent any
	tools {
		maven 'localMaven'
	}

	parameters{
		string (name: 'staging-tomcat', defaultValue: 'localhost', description: 'Staging server')
		string (name: 'prod-tomcat', defaultValue: 'localhost', description: 'Production server')
	}

	triggers {
		pollSCM('* * * * *')
	}

	stages{
		stage('Build'){
			steps{
				sh 'mvn clean package'
			}
			post {
				success{
					echo 'Now Archiving'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}

		stage('Deployments'){
			parallel{
				stage('Deploy to Staging'){
					steps{
						sh "cp -i **/target/*.war /Users/kerimdjiho/Documents/Workshop/apache-tomcat-8.5.29-staging/webapps -y"
					}
				}
				stage('Deploy to Prod'){
					steps{
						sh "cp -i **/target/*.war /Users/kerimdjiho/Documents/Workshop/apache-tomcat-8.5.29-prod/webapps -y"
					}
				}
			}
		}
	}
}