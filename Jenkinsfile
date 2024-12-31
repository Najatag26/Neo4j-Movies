pipeline {
    agent any

    stages {
        stage('Cloner le dépôt Git') {
            steps {
                git credentialsId: 'git-ssh-key', url: 'git@github.com:Najatag26/Automatisation_Project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Tests') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
