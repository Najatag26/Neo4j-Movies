pipeline {
    agent any

    stages {
        stage('Cloner le dépôt Git') {
            steps {
                git branch: 'main', credentialsId: 'git-ssh-key', url: 'git@github.com:Najatag26/Automatisation_Project.git'
            }
        }
        stage('Build') {
            steps {
                sh './gradlew build' // Ou utilisez mvn si Maven est utilisé
            }
        }
        stage('Tests') {
            steps {
                sh './gradlew test' // Exécuter les tests
            }
        }
    }
}
