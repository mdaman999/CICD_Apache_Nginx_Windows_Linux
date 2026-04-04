pipeline {
    agent any

    // 1. Ye block Webhook trigger ko fix karega
    triggers {
        githubPush()
    }

    stages {
        stage('Cleanup') {
            steps {
                // Purani files delete karega taaki fresh code deploy ho
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'Apache_Windows') {
                        echo "Deploying to Apache on Windows..."
                        bat 'xcopy /s /y * "C:\\Apache24\\htdocs\\"'

                    } else if (env.BRANCH_NAME == 'Apache_Linux') {
                        echo "Deploying to Apache on WSL (Ubuntu)..."
                        // WSL commands
                        bat 'wsl -u root rm -rf /var/www/html/*'
                        bat 'wsl -u root cp -r . /var/www/html/'
                        echo "Visit: http://localhost:82"

                    } else if (env.BRANCH_NAME == 'Nginx_Windows') {
                        echo "Deploying to Nginx on Windows..."
                        bat 'xcopy /s /y * "C:\\nginx-1.28.3\\html\\"'

                    } else if (env.BRANCH_NAME == 'Nginx_Linux') {
                        echo "Deploying to Nginx on WSL (Ubuntu)..."
                        bat 'wsl -u root rm -rf /var/www/nginxhtml/*'
                        bat 'wsl -u root cp -r . /var/www/nginxhtml/'
                        echo "Visit: http://localhost:84"

                    } else {
                        echo "Branch '${env.BRANCH_NAME}' ke liye koi rule nahi hai."
                    }
                }
            }
        }
    }

    // 3. Status updates (optional but helpful)
    post {
        success {
            echo "Successfully deployed branch: ${env.BRANCH_NAME}"
        }
        failure {
            echo "Build failed! Logs check karein."
        }
    }
}
