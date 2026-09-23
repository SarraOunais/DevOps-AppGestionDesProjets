pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SarraOunais/DevOps-AppGestionDesProjets.git'
            }
        }

        stage('Test') {
            steps {
                echo 'Projet récupéré depuis GitHub avec succès !'
            }
        }
    }
}
