pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'java17'
    }

    environment {
        DOCKER_IMAGE = "yourdockerhub/petclinic:latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/Suspicious-lab/Petclinicneww.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }

    post {
        success {
            slackSend message: "✅ Petclinic deployed successfully!"
        }
        failure {
            slackSend message: "❌ Petclinic pipeline failed!"
        }
    }
}
