pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    dir("${env.BRANCH_NAME}") {
                        checkout scm
                        echo "Code checked out in folder: ${env.BRANCH_NAME}"
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    dir("${env.BRANCH_NAME}") {
                        if (env.BRANCH_NAME == 'Apache_Win') {
                            echo "Deploying code to apache web server on windows 11. Can visit http://localhost:81"
                            // Example command: bat 'copy * C:\\Apache24\\htdocs\\'

                        } else if (env.BRANCH_NAME == 'Apache_Linux') {
                            echo "Deploying code to apache web server on Ubuntu. Can visit http://localhost:82"
                            // Example command: sh 'cp -r * /var/www/html/'

                        } else if (env.BRANCH_NAME == 'Nginx_Win') {
                            echo "Deploying code to nginx web server on windows 11. Can visit http://localhost:83"

                        } else if (env.BRANCH_NAME == 'Nginx_Linux') {
                            echo "Deploying code to nginx web server on Ubuntu. Can visit http://localhost:84"

                        } else {
                            echo "Building other branch: ${env.BRANCH_NAME}"
                        }
                    }
                }
            }
        }
    }
}
