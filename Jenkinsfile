pipeline {
		agent any
		stages {
				stage("Checkout code") {
						steps {
								checkout scm
						}
				}
				stage("Build image") {
						steps {
								script {
										myapp = docker.build("raniasaleh/web-dvwa:${env.BUILD_ID}")
								}
						}
				}
				stage("Push image") {
						steps {
								script {
										docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
														myapp.push("latest")
														myapp.push("${env.BUILD_ID}")
										}
								}
						}
				}
				stage('Deploy to ibmcloud') {
						steps{
							 sh '''
								 #!/bin/bash
								kubectl apply -f deploy_vuln_app.yaml -n test --validate=false
								'''
						}
				}
		}
}
