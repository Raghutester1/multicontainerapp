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
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-authtoken') {
                        docker.image("raghuramdevopsengineer/frontend:${env.BUILD_NUMBER}").push()
                        docker.image("raghuramdevopsengineer/backend:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        // stage('Deploy to Kubernetes') {
        //     steps {
        //         script {
        //             sh 'kubectl apply -f k8s-deployment.yaml'
        //         }
        //     }
        // }
    }
}