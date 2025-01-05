pipeline {
    agent any

    environment {
        // Jeton d'authentification pour SonarQube
        SONAR_TOKEN = credentials('sonar-token')
    }

    tools {
        // Configuration Maven (nom configuré dans Jenkins)
        maven 'Maven'
    }

    stages {
        // Étape pour cloner le projet depuis GitHub
        stage('Cloner le projet') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/Najatag26/Neo4j-Movies.git' // URL du dépôt
            }
        }

        // Étape pour exécuter l'analyse SonarQube
        stage('Analyse SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    // Commande pour exécuter l'analyse avec SonarQube
                    sh "mvn clean verify sonar:sonar -Dsonar.projectKey=Neo4j-Movies -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        // Étape pour attendre la Quality Gate
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') { // Temps d'attente maximum pour la qualité
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
