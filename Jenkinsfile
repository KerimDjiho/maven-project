pipeline {
	/* A declarative Pipeline */
	agent any
	tools {
		maven 'localMaven'
	}

	parameters{
		string (name: 'staging-tomcat', defaultValue: 'localhost:8090', description: 'Staging server')
		string (name: 'prod-tomcat', defaultValue: 'localhost:9080', description: 'Production server')
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
						sh "cp **/target/*.war ${params.staging-tomcat}:Users/kerimdjiho/Documents/Workshop/apache-tomcat-8.5.29-staging/webapps"
					}
				}
				stage('Deploy to Prod'){
					steps{
						sh "cp **/target/*.war ${params.prod-tomcat}:/Users/kerimdjiho/Documents/Workshop/apache-tomcat-8.5.29-prod/webapps"
					}
				}
			}
		}
	}
}