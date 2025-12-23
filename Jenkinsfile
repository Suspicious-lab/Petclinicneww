pipeline {
    agent any

    tools {
        jdk 'JAVA'
        maven 'MAVEN'
    }

    environment {
        IMAGE_NAME = "suspicious08/petclinic"
    }

    stages {
        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                withDockerRegistry(
                  credentialsId: 'dockerhub-creds',
                  url: 'https://index.docker.io/v1/'
                ) {
                    sh '''
                    docker build -t $IMAGE_NAME:latest .
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }
    }
}
