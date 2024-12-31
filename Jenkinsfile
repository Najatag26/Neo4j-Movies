pipeline {
    agent any

    stages {
        stage('Cloner le dépôt Git') {
            steps {
                // Cloner le projet depuis le dépôt Git
                git 'https://github.com/Najatag26/Automatisation_Project.git'
            }
        }

        stage('Analyse SonarQube') {
            steps {
                // Analyse avec SonarQube
                sh 'sonar-scanner'
            }
        }

        stage('Build du projet') {
            steps {
                // Construire le projet (gradle ou maven selon votre projet)
                sh './gradlew build' // Remplacez par `mvn clean install` si vous utilisez Maven
            }
        }

        stage('Créer une image Docker') {
            steps {
                // Construire une image Docker
                sh 'docker build -t nom_utilisateur/nom_image:latest .'
            }
        }

        stage('Pousser l’image Docker sur Docker Hub') {
            steps {
                // Pousser l’image sur Docker Hub
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push nom_utilisateur/nom_image:latest'
                }
            }
        }

        stage('Déployer le conteneur Docker') {
            steps {
                // Exécuter le conteneur Docker
                sh 'docker run -d -p 8080:8080 nom_utilisateur/nom_image:latest'
            }
        }
    }
}
