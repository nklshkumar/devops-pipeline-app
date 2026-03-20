pipeline {
    agent {
        docker {
            image 'node:18'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        IMAGE_NAME = "devops-app"
    }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'cd app && npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'cd app && npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker rm -f devops-app || true'
                sh 'docker run -d -p 3000:3000 --name devops-app $IMAGE_NAME'
            }
        }
    }
}
