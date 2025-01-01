pipeline {
    agent any

    stages {
        stage('Cloner le dépôt Git') {
            steps {
               git branch: 'main', credentialsId: '19dfc1cb-3cab-444f-828c-ba259cdcff03', url: 'git@github.com:Najatag26/Automatisation_Project.git'

            }
        }

        stage('Analyse SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube Scanner' // Assurez-vous que SonarQube Scanner est installé
                    sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=Automatisation_Project \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=SonarQubeToken
                    """
                }
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
