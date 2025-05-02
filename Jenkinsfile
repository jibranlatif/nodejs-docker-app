pipeline {
    agent any

    environment {
        IMAGE_NAME = "nodejs-docker-app"
        CONTAINER_NAME = "node-app"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-creds',
                    branch: 'main',
                    url: 'https://github.com/jibranlatif/nodejs-docker-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop and Remove Existing Container') {
            steps {
                sh '''
                if [ $(docker ps -q -f name=$CONTAINER_NAME) ]; then
                    docker stop $CONTAINER_NAME
                fi
                if [ $(docker ps -aq -f name=$CONTAINER_NAME) ]; then
                    docker rm $CONTAINER_NAME
                fi
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name $CONTAINER_NAME $IMAGE_NAME'
            }
        }
    }
}

