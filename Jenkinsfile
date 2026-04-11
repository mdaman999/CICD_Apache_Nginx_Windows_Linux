pipeline {
    agent any
	
	environment {
        DOCKER_HUB_USER = 'mdaman999'
        IMAGE_NAME = 'my-nginx-image'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        DOCKER_CRED_ID = 'docker-credential'
    }

	triggers {
        githubPush()
    }
	
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
		
        stage('Deploy') {
            steps {
                script {
					switch(env.BRANCH_NAME) {
						case 'Apache_Windows':
							echo "Deploying to Apache on Windows..."
							bat 'xcopy /y "index.html" "C:\\Apache24\\htdocs\\"'
							echo "Visit: http://localhost:81"
							break
							
						case 'Apache_Linux':
							echo "Deploying to Apache on WSL (Ubuntu)..."
							bat 'wsl -u root cp -f index.html /var/www/ApacheHTML/index.html'
							echo "Visit: http://localhost:82"
							break
							
						case 'Apache_K8s':
							deployK8s('my-apache-deployment')
							break
							
						case 'Nginx_Windows':
							echo "Deploying to Nginx on Windows..."
							bat 'xcopy /y "index.html" "C:\\nginx\\html\\"'
							echo "Visit: http://localhost:83"
							break
							
						case 'Nginx_Linux':
							echo "Deploying to Nginx on WSL (Ubuntu)..."
							bat 'wsl -u root cp -f index.html /var/www/NginxHTML/index.html'
							echo "Visit: http://localhost:84"
							break
							
						case 'Nginx_K8s':
							deployK8s('my-nginx-deployment')
							break
							
						default:
							echo "No deployment rule for branch: ${env.BRANCH_NAME}"
					}
                }
            }
        }
    }
}

def deployK8s(deploymentName) {
    echo "Building and pushing specific version: ${IMAGE_TAG}"
	withCredentials([usernamePassword(credentialsId: "${DOCKER_CRED_ID}", passwordVariable: 'PASS', usernameVariable: 'USER')]) 
	{
		bat "docker login -u %USER% -p %PASS%"
		bat "docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ."
		bat "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
	}
    echo "Updating K8s deployment: ${deploymentName} with image tag: ${IMAGE_TAG}"
    bat "kubectl set image deployment/${deploymentName} nginx=${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
    bat "kubectl rollout status deployment/${deploymentName}"
}
