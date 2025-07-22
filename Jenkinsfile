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
                    sh 'pip install -r requirements.txt'
                    sh 'python app.py & sleep 5'
                    sh 'curl http://localhost:5000/health'
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
