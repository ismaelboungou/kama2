pipeline {
    agent any

    environment {
        GITHUB_REPO = "ismaelboungou/kama2"
        DOCKER_IMAGE = "${env.GITHUB_REPO}:${env.BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    docker.build("${env.DOCKER_IMAGE}")
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'GITHUB_USERNAME', passwordVariable: 'GITHUB_TOKEN')]) {
                        sh "echo ${GITHUB_TOKEN} | docker login ghcr.io -u ${GITHUB_USERNAME} --password-stdin"
                        sh "docker push ${env.DOCKER_IMAGE}"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose down'
                sh "docker-compose up -d --build"
            }
        }
    }
}
