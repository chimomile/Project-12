pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                script {
                    // Build Docker image dengan tag "test-image"
                    docker.build('test-image')
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo 'Running Docker container...'
                script {
                    // Menjalankan container sementara dari image "test-image"
                    def container = docker.image('test-image').run('-d')
                    sh 'docker ps' // Menampilkan daftar container yang berjalan
                }
            }
        }

        stage('Clean Up') {
            steps {
                echo 'Cleaning up Docker resources...'
                script {
                    // Menghapus container dan image setelah pengujian
                    sh 'docker rm -f $(docker ps -aq)'
                    sh 'docker rmi -f test-image'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline test completed.'
        }
    }
}
