pipeline {
    agent {
        node {label "UAT"}
    }

    tools {
        dockerTool 'docker'
    }
    environment {
        IMAGE_NAME = 'cloud1111/jenkins-flask-app-demo'
        IMAGE_TAG = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
        // KUBECONFIG = credentials('kubeconfig-credentials-id')
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')
        AWS_SECRET_ACCESS_KEY_ID = credentials('aws-secret-key')
        
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
                withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
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
        stage('Deploy to EKS Cluster') {
            steps {
                sh " aws eks update-kubeconfig --region us-east-1 --name CastAI-POC-EKS-Cluster"
                sh "kubectl apply -f deployment.yaml -n jenkins"
                echo "Deployed to EKS Cluster"
            }
        }
    }
}
