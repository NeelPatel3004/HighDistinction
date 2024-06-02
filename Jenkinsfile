pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/your-username/my-node-app.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.build('my-node-app:latest')
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image('my-node-app:latest').inside {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.image('my-node-app:latest').run('-p 3000:3000')
                }
            }
        }
    }

    post {
        always {
            junit 'reports/*.xml'
        }
        failure {
            mail to: 'team@example.com',
                 subject: "Pipeline Failed: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.BUILD_URL}"
        }
    }
}
