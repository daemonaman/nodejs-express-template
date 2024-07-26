
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-nginx-app'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/daemonaman/nodejs-express-template.git'
            }
        }
    stage ("Build the code"){
			steps{
				sh 'npm install'
			}
		}

        
    stage('Remove Old Containers and images') {
            steps {
                script {
                    sh '''
                    sudo docker stop nginx || true
                    sudo docker rm nginx || true
                    '''
                    sh '''
                    sudo docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true
                    '''
                }
            }
        }


        stage('Build') {
            steps {
                script {
                    sh 'sudo docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh '''
                    sudo docker run -dit --name nginx -p 8081:80 ${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }
    }
}
