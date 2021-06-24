pipeline {
	agent any
		 environment {
       			docker_host = '10.1.2.34'
		}
		stages {
			stage('Slack Message') {
            			steps {
                			slackSend channel: '#python',
                    			color: 'good',
                    			message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}"
            
            			}
        		}
			stage ( 'Cloneing github' ){
				steps {
					sh 'rm -rf dockertest1'
					sh 'git clone --branch newjenkins https://github.com/ferozbhai/dockertest1.git'
				}
			}
			stage ( 'Build Docker Image' ){
				steps {
					sh 'cd /var/lib/jenkins/workspace/pipline1/dockertest1'
					sh 'cp /var/lib/jenkins/workspace/pipline1/dockertest1/* /var/lib/jenkins/workspace/pipline1'
					sh 'docker build -t ferozshaik/piplinetest1:${BUILD_NUMBER} .'
				}
			}
			stage ( 'push image to docker hub' ){
				steps {
					sh 'docker push ferozshaik/piplinetest1:${BUILD_NUMBER}'
					
				}
			}
			stage ( 'Deploy to Docker Host' ){
				steps {
					sh 'docker -H tcp://$docker_host:2375 stop prodwebapp1 || true'
					sh 'docker -H tcp://$docker_host:2375 run --rm -dit --name prodwebapp1  --hostname prodwebapp1 -p 8000:80 ferozshaik/piplinetest1:${BUILD_NUMBER}'
					
				}
			}
			stage ( 'Check webapp Reachabulity' ){
				steps {
					sh 'sleep 10s'
					sh 'curl http://$docker_host:8000'
				}
			
			}
		

	}
}
