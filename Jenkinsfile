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
                    // Jenkins credentials ko environment variables mein map karna
                    withCredentials([usernamePassword(credentialsId: 'ubuntu_server_creds', passwordVariable: 'UBUNTU_PASS', usernameVariable: 'UBUNTU_USER')]) {
                        
                        if (env.BRANCH_NAME == 'Apache_Windows') {
                            echo "Deploying to Apache on Windows..."
                            bat 'xcopy /s /y * "C:\\Apache24\\htdocs\\"'

                        } else if (env.BRANCH_NAME == 'Apache_Linux') {
                            echo "Deploying to Apache on Ubuntu via SSH/WSL..."
                            bat "wsl printf '%s\n' ${UBUNTU_PASS} | sudo -S rm -rf /var/www/html/*"
                            bat "wsl printf '%s\n' ${UBUNTU_PASS} | sudo -S cp -r . /var/www/html/"
                            echo "Visit: http://localhost:82"

                        } else if (env.BRANCH_NAME == 'Nginx_Windows') {
                            echo "Deploying to Nginx on Windows..."
                            bat 'xcopy /s /y * "C:\\nginx-1.28.3\\html\\"'

                        } else if (env.BRANCH_NAME == 'Nginx_Linux') {
                            echo "Deploying to Nginx on Ubuntu via SSH/WSL..."
                            bat "wsl printf '%s\n' ${UBUNTU_PASS} | sudo -S rm -rf /var/www/nginxhtml/*"
                            bat "wsl printf '%s\n' ${UBUNTU_PASS} | sudo -S cp -r . /var/www/nginxhtml/"
                            echo "Visit: http://localhost:84"

                        } else {
                            echo "No rule for ${env.BRANCH_NAME}"
                        }
                    }
                }
            }
        }
    }
}
