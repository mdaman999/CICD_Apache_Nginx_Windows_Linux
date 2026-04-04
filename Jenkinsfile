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
                   if (env.BRANCH_NAME == 'Apache_Win') {
                        echo "Deploying code to apache web server on windows 11. Can visit http:/localhost:81"

                    } else if (env.BRANCH_NAME == 'Apache_Linux') {
                        echo "Deploying code to apache web server on Ubantu. Can visit http:/localhost:82"

                    } else if (env.BRANCH_NAME == 'Nginx_Win') {
                        echo "Deploying code to nginx web server on windows 11. Can visit http:/localhost:83"

                    } else if (env.BRANCH_NAME == 'Nginx_Linux') {
                        echo "Deploying code to nginx web server on Ubantu. Can visit http:/localhost:84"

                    } else {
                        echo "Building other branch: ${env.BRANCH_NAME}"
                    }
                }
            }
        }
    }
}
