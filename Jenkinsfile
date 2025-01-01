pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "techikariskasari/project-12:latest"
        DOCKER_CONTAINER = "project-12"
        PORT_MAPPING = "8089:80"
    }
    
    stages {
        stage("Clone Repository") {
            steps {
                // Clean workspace before cloning
                cleanWs()
                checkout([$class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[url: 'https://github.com/chimomile/Project-12.git']]
                ])
            }
        }
        
        stage("Build Docker Image") {
            steps {
                script {
                    bat "docker build -t ${env.DOCKER_IMAGE} ."
                }
            }
        }
        
        stage("Clean Previous Container") {
            steps {
                script {
                    // Using Windows batch commands
                    bat """
                        docker ps -q --filter name=${env.DOCKER_CONTAINER} && docker stop ${env.DOCKER_CONTAINER} || exit 0
                        docker ps -aq --filter name=${env.DOCKER_CONTAINER} && docker rm ${env.DOCKER_CONTAINER} || exit 0
                    """
                }
            }
        }
        
        stage("Run Docker Container") {
            steps {
                script {
                    bat "docker run -d --name ${env.DOCKER_CONTAINER} -p ${env.PORT_MAPPING} ${env.DOCKER_IMAGE}"
                }
            }
        }
    }
    
    post {
        always {
            // Clean up old images
            bat "docker image prune -f"
        }
        failure {
            echo 'Pipeline failed!'
        }
        success {
            echo 'Pipeline succeeded! Website deployed.'
        }
    }
}
