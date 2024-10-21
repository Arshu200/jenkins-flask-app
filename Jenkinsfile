pipeline {
    agent any
    // tools {
    //     dockerTool 'docker'
    // }
    environment {
        IMAGE_NAME = 'arshad200/jenkins-flask-app-demo'
        IMAGE_TAG = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
    }
    stages {
        stage('Test') {
            steps {
                echo "Test Stage"
                sh "whoami"
            }
        }
        stage('Login to docker hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                sh 'sudo docker login -u ${USERNAME} -p ${PASSWORD}'}
                echo 'Login successfully'
            }
        }
        stage('Build Docker Image')
        {
            steps
            {
                sh 'sudo docker build -t ${IMAGE_TAG} .'
                echo "Docker image build successfully"
                sh "sudo docker images"
            }
        }
        stage('Push Docker Image')
        {
            steps
            {
                sh 'sudo docker push ${IMAGE_TAG}'
                echo "Docker image push successfully"
            }
        }
    }
}
