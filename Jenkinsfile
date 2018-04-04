pipeline {
	/* A declarative Pipeline */
	agent any
	tools {
		maven 'localMaven'
	}

	parameters{
		string (name: 'staging-tomcat', defaultValue: '172.0.0.1:8090', description: 'Staging server')
		string (name: 'prod-tomcat', defaultValue: '172.0.0.1:9080', description: 'Production server')
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
						sh "cp -i **/target/*.war ${params.staging-tomcat}:../apache-tomcat-8.5.29-staging/webapps"
					}
				}
				stage('Deploy to Prod'){
					steps{
						sh "cp -i **/target/*.war ${params.prod-tomcat}:../apache-tomcat-8.5.29-prod/webapps"
					}
				}
			}
		}
	}
}