pipeline {
    agent any
	
	environment {
        DOCKER_HUB_USER = 'mdaman999'
        IMAGE_NAME = 'my-nginx-img'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        DOCKER_CRED_ID = 'dockercreds'
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

        stage('Build & Push image to Docker Hub') {
            steps {
                script {
                    // 1. Docker Build
                    bat "docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ."
                    bat "docker tag ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
                    
                    // 2. Docker Push (Jenkins mein Docker Hub credentials save hone chahiye)
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CRED_ID}", passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        bat "docker login -u %USER% -p %PASS%"
                        bat "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                        bat "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
                    }
                }
            }
        }
		
        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'Apache_Windows') {
                        echo "Deploying to Apache on Windows..."
                        bat 'xcopy /s /y * "C:\\Apache24\\htdocs\\"'
						echo "Visit: http://localhost:81"

                    } else if (env.BRANCH_NAME == 'Apache_Linux') {
                        echo "Deploying to Apache on WSL (Ubuntu)..."
                        bat 'wsl -u root rm -rf /var/www/html/*'
                        bat 'wsl -u root cp -r . /var/www/html/'
                        echo "Visit: http://localhost:82"

                    } else if (env.BRANCH_NAME == 'Nginx_Windows') {
                        echo "Deploying to Nginx on Windows..."
                        bat 'xcopy /s /y * "C:\\nginx-1.28.3\\html\\"'
						echo "Visit: http://localhost:83"

                    } else if (env.BRANCH_NAME == 'Nginx_Linux') {
                        echo "Deploying to Nginx on WSL (Ubuntu)..."
                        bat 'wsl -u root rm -rf /var/www/nginxhtml/*'
                        bat 'wsl -u root cp -r . /var/www/nginxhtml/'
                        echo "Visit: http://localhost:84"

                    } else if (env.BRANCH_NAME =='K8s_Deploy' ){
					    echo "Deploying nginx image to kubernate cluster..."
						bat "kubectl set image deployment/my-nginx nginx=${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
						bat "kubectl rollout status deployment/my-nginx"
						echo "K8s Deployment Successful!"
					} else {
                        echo "No rule for ${env.BRANCH_NAME}"
                    }
                }
            }
        }
    }
}
