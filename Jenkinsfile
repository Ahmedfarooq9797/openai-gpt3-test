pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "openai-gpt3"
    }

    stages {
        stage('Clone repository') {
            steps {
                git 'https://github.com/Ahmedfarooq9797/openai-gpt3-test.git'
            }
        }

        stage('Build Docker image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Run Docker container') {
            steps {
                script {
                    docker.image(DOCKER_IMAGE).run('-it -p 3000:3000')
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up Docker containers and images
                def containers = sh(script: "docker ps -aq -f ancestor=${DOCKER_IMAGE}", returnStdout: true).trim()
                if (containers) {
                    sh "docker rm -f ${containers}"
                }
                def images = sh(script: "docker images -q ${DOCKER_IMAGE}", returnStdout: true).trim()
                if (images) {
                    sh "docker rmi -f ${images}"
                }
            }
        }
    }
}
