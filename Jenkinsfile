pipeline {
    agent any
    stages {
        stage('Build and Push Docker Images') {
            steps {
                script {
                    docker.build("raghuramdevopsengineer/react:${env.BUILD_NUMBER}", "./react")
                    docker.build("raghuramdevopsengineer/node:${env.BUILD_NUMBER}", "./node")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    bat 'docker logout'
                    bat 'del C:\\Users\\raghuram\\.docker\\config.json'
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-authtoken') {
                        docker.image("raghuramdevopsengineer/react:${env.BUILD_NUMBER}").push()
                        docker.image("raghuramdevopsengineer/node:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // bat 'kubectl set image deployment/node-deployment node=raghuramdevopsengineer/react:${env.BUILD_NUMBER}'
                    // bat 'kubectl set image deployment/react-deployment react=raghuramdevopsengineer/node:${env.BUILD_NUMBER}'
                    bat 'kubectl apply -f k8s-deployment.yaml'
                }
            }
        }
    }
}