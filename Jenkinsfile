pipeline {
    agent any
    
    environment {
        IMAGE_NAME = "maven"
        CONTAINER_NAME = "jenkins-java-container"
        PATH = "/opt/homebrew/bin:/usr/local/bin/docker:${env.PATH}"
        DOCKERHUB_USERNAME = "navyatatikonda7"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/navya-tatikonda/Project3.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                /usr/local/bin/docker build -t $DOCKERHUB_USERNAME/$IMAGE_NAME .
                '''
            }
         }


        stage('Deploy Docker Container') {
            steps {
                sh '''
                /usr/local/bin/docker stop $CONTAINER_NAME || true
                /usr/local/bin/docker rm $CONTAINER_NAME || true
                /usr/local/bin/docker run -d \
                --name $CONTAINER_NAME \
                -p 8081:8080 \
                $.DOCKERHUB_USERNAME/$IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment successful 🚀'
        }
        failure {
            echo 'Deployment failed ❌'
        }
    }
}
