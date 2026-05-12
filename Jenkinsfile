pipeline {
    agent any

    environment {
        IMAGE_NAME = "django-python-app"
        CONTAINER_NAME = "django-container"
        PORT = "8000"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/sreenadhcs/python-django.git'
            }
        }

        stage('Verify Files') {
            steps {
                sh '''
                echo "Project Files:"
                ls -la
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d \
                --name $CONTAINER_NAME \
                -p 8000:8000 \
                $IMAGE_NAME
                '''
            }
        }

        stage('Check Running Container') {
            steps {
                sh '''
                docker ps
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment Successful'
        }

        failure {
            echo 'Pipeline Failed'
        }
    }
}
