pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/neerajkheria/devops-healthcheck-portal.git'
            }
        }

        stage('Build backend') {
            steps {
                dir('backend') {
                    bat 'pip install -r requirements.txt'
                    bat 'start /B python app.py & timeout T/5'
                    bat 'curl http://localhost:5000/health'
                }
            }
        }

        stage('Test Backend') {
            steps {
                dir('backend') {
                    sh '''
                    response=$(curl -s http://localhost:5000/health)
                    if [["$response" != *"status"]]; then
                        echo "Health Check failed!"
                        exit 1
                    fi
                    '''
                }
            }
        }
   }
}
