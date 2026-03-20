pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-app"
    }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'docker run --rm -v $PWD:/workspace -w /workspace/app node:18 npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'docker run --rm -v $PWD:/workspace -w /workspace/app node:18 npm test'
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
