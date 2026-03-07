pipeline {
    agent any

    environment {
        GIT_REPO        = "https://github.com/shivaprabha2997/react_js.git"
        GIT_BRANCH      = "main"

        DOCKERHUB_USER  = "shivadocker2997"
        IMAGE_NAME      = "react-app"
        IMAGE_TAG       = "${BUILD_NUMBER}"

        DOCKER_CREDS    = "Docker_CRED"

        CONTAINER_NAME  = "react-container"
        HOST_PORT       = "8099"
        CONTAINER_PORT  = "80"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'shiva_git', url: 'https://github.com/shivaprabha2997/react_js.git']])
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} .
                """
            }
        }

        stage('DockerHub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKER_CREDS}",
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh """
                    echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                    """
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh """
                docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true

                docker run -d \
                -p ${HOST_PORT}:${CONTAINER_PORT} \
                --name ${CONTAINER_NAME} \
                ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }
    }
}
