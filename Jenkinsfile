pipeline {
	agent any

	parameters {
		string(name: 'tomcat_dev', defaultValue: '52.67.122.24', description: 'Staging Server')
		string(name: 'tomcat_prod', defaultValue: '18.228.30.9', description: 'Production Server')
	}

	triggers {
		pollSCM('* * * * *')
	}

	stages{
		stage('Build'){
			steps {
				sh 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}

		stage ('Deployments'){
			parallel{
				stage ('Deploy to Staging'){
					steps {
						sh "scp -i /home/kenneth/Documents/MyEC2KeyPair.pem -o StrictHostKeyChecking=no **/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
					}
				}

				stage ("Deploy to Production"){
					steps {
						sh "scp -i /home/kenneth/Documents/MyEC2KeyPair.pem -o StrictHostKeyChecking=no **/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
					}
				}
			}
		}
	}
}
