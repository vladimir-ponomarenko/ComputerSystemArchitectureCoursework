pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("local/repository:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh '''
                        docker stop app-container || true
                        docker rm app-container || true
                    '''
                    sh '''
                        docker run -d --name app-container -p 8080:8080 local/repository:${env.BUILD_NUMBER}
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
