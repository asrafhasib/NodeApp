pipeline {
	agent any
	tools {
		nodejs 'Nodejs'
	}
	environment {
		DOCKER_HUB_CREDENTIALS_ID = 'jen-dockerhub'
		DOCKER_HUB_REPO = 'asrafhasib/nodejs-app'
	}
	stages {
		stage('Checkout Github'){
			steps {
				git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/asrafhasib/NodeApp.git'
			}
		}		
		stage('Install node dependencies'){
			steps {
				sh 'npm install'
			}
		}
		stage('Test Code'){
			steps {
				sh 'npm test'
			}
		}
		stage('Build Docker Image'){
			steps {
				script {
					dockerImage = docker.build("${DOCKER_HUB_REPO}:latest")
				}
			}
		}
		stage('Install Kubectl'){
			steps {
				sh '''
				curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                		chmod +x kubectl
                		mv kubectl /usr/local/bin/kubectl
				'''
			}
		}
		
		stage('Kubernetes Deploy'){
			steps {
				script {
				kubeconfig(caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVEND QWUyZ0F3SUJBZ0lJZmRWVG5YbkR0TjR3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhN S2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRFd01ERXhNVEV4TURsYUZ3MHpOREE1TWpreE1URTJNRGxhTUJV eApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3 QXdnZ0VLCkFvSUJBUUM4dzBEdzhaT0cyRkl6eFZidWVDeDlkUFhJT3lnbC9Fa3gyZXZwUWZHYThDWVNu clBBTGpPbUFodWgKdUJrR21iUXFmWHRCSSs0T1JybmRGd2lpL2NpZ21RSnR5ZU04RGxZMTk5S29YRXJa TTBnL0tqeWpHVEV2OGdWYgoyZXllUWs5blQrdnRDaHZVQkgycGdwSEhKbmRrSnhZS0FvejhzbnFFWlI2 OEY1Q1YzZzdsS1pLcXhQWklRaCtVCjZCdXdqRWRwUDd0QjJYais5R1VFVDZEYmUzd2UvaTZWT21TUDg2 cU5qUExPKzFWa0dBRzRUeGUyMEpXY0t2a20KU2hwTStjcmZnbTRTZVVpNWRhK0k3NURHSjdzTmV1dVZL cFk3aTUybWJQL0ZWR2lUcWtEQlhGNkR1VVZiUXRxawozUEFmQmxZektSVHBwWDZlYVloU0tFcnlsR1lO QWdNQkFBR2pXVEJYTUE0R0ExVWREd0VCL3dRRUF3SUNwREFQCkJnTlZIUk1CQWY4RUJUQURBUUgvTUIw R0ExVWREZ1FXQkJTS2QxTnlQeDZhaXdMQkpVTGVJdVNCUVBxLzZUQVYKQmdOVkhSRUVEakFNZ2dwcmRX SmxjbTVsZEdWek1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRQ296amp2dndmcQp6eHlMQTRLUzhHc25z UEcvUU5CeTl3c2haSFI1NzhPU0YyZkZvRXh6Y3hSM0Z5RFJwYityeUpVOHZwQlVBN1VhCkFoeDVWeEpF SVZVY3BXTjdtWC93RlI0V0xqRjVmaWZJZnRXcURndXE3ZFFxR2ZyaHhCbUVadFU3MFlVRCsyTDYKeG1Q RWl1L28wbGlCTHJFeGlRWXRwaU8xVFkxTHNjS3VtaExDN1ZEanlaNWd0KzF5bThNZUgrMEZlemM1WmFL eApPajJrcGxrZmdqaURnMW0rekxVMWtTSXJGZWo4a04ybnVOT1YvaDFvWGdETDdMNmdoYllBajh1TzRI UHg2L2ZQCmxtczZTMVczWDdtMVNtYkFyR0Q0S2o5OUgzVGIzaXMzV2xSTXlhZndLM2VaWndEVzNQenR4 QUFvS2FydW1tVncKaEdRWDdFcGJ5azJFCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K', credentialsId: 'kubeconfig', serverUrl: 'https://192.168.49.2:8443')
					{
				sh 'kubectl apply -f deployment.yaml' 
					}
				}
	                }
                }
	}
}
