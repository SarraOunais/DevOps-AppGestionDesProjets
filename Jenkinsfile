pipeline {
    agent any

    environment {
        DOCKER_USER = 'sarraboh'
        BACKEND_IMAGE = "${DOCKER_USER}/backend"
        FRONTEND_IMAGE = "${DOCKER_USER}/frontend"
        IMAGE_TAG = "1.0"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Recuperation du projet depuis GitHub...'

                git branch: 'main',
                    url: 'https://github.com/SarraOunais/DevOps-AppGestionDesProjets.git'
            }
        }

        stage('Test') {
            steps {
                echo 'Verification du projet...'

                sh '''
                    echo "Projet recupere avec succes"
                    ls -la
                    echo "Backend :"
                    ls -la backend
                    echo "Frontend :"
                    ls -la frontend
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Construction des images Docker...'

                sh '''
                    docker build -t ${BACKEND_IMAGE}:${IMAGE_TAG} ./backend
                    docker build -t ${FRONTEND_IMAGE}:${IMAGE_TAG} ./frontend
                '''
            }
        }

        stage('Docker Login') {
            steps {
                echo 'Connexion a Docker Hub...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                            -u "$DOCKER_USERNAME" \
                            --password-stdin
                    '''
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Envoi des images vers Docker Hub...'

                sh '''
                    docker push ${BACKEND_IMAGE}:${IMAGE_TAG}
                    docker push ${FRONTEND_IMAGE}:${IMAGE_TAG}
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline termine avec succes !'
            echo 'Les images Docker ont ete envoyees vers Docker Hub.'
        }

        failure {
            echo 'Le pipeline a echoue.'
        }
    }
}
