pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/vladimir-ponomarenko/ComputerSystemArchitectureCoursework.git'
            }
        }

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
                    dockerImage.inside {
                        sh '''
                            docker stop app-container || true
                            docker rm app-container || true
                            docker run -d --name app-container -p 8080:8080 local/repository:${env.BUILD_NUMBER}
                        '''
                    }
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
