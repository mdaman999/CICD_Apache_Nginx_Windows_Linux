pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'Apache_Windows') {
                        echo "Deploying to Apache on Windows 11..."
                        bat 'xcopy /s /y * "C:\\Apache24\\htdocs\\"'
                        echo "Visit: http://localhost:81"

                    } else if (env.BRANCH_NAME == 'Apache_Linux') {
                        echo "Deploying to Apache on WSL (Ubuntu)..."
                        bat 'wsl sudo rm -rf /var/www/html/*'
                        bat 'wsl sudo cp -r . /var/www/html/'
                        echo "Visit: http://localhost:82"

                    } else if (env.BRANCH_NAME == 'Nginx_Windows') {
                        echo "Deploying to Nginx on Windows 11..."
                        bat 'xcopy /s /y * "C:\\nginx-1.28.3\\html\\"'
                        echo "Visit: http://localhost:83"

                    } else if (env.BRANCH_NAME == 'Nginx_Linux') {
                        echo "Deploying to Nginx on WSL (Ubuntu)..."
                        bat 'wsl sudo rm -rf /var/www/nginxhtml/*'
                        bat 'wsl sudo cp -r . /var/www/nginxhtml/'
                        echo "Visit: http://localhost:84"

                    } else {
                        echo "No deployment rule for branch: ${env.BRANCH_NAME}"
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo "Deployment successful on ${env.BRANCH_NAME}!"
        }
        failure {
            echo "Deployment failed. Please check Jenkins logs."
        }
    }
}
