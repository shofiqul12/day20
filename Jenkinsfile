pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'shofiqul12/myapp-day-20:latest'
        DOCKERHUB_CREDENTIALS = 'dockerhub-user-pass' // আপনার credentials ID
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            agent {
                docker { image 'docker:24.0.0-cli' }
            }
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to DockerHub') {
            agent {
                docker { image 'docker:24.0.0-cli' }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: "$DOCKERHUB_CREDENTIALS", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploy stage: আপনার সার্ভারে push করা Docker image deploy করুন"
                // এখানে আপনি SSH, kubectl, বা docker run commands ব্যবহার করতে পারেন
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
