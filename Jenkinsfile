pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token') // Jeton d'authentification
    }

    tools {
        maven 'Maven' // Maven configuré dans Jenkins
    }

    stages {
        stage('Cloner le projet') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/Najatag26/Neo4j-Movies.git'
            }
        }

        stage('Analyse SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    sh """
                    mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=Neo4j-Movies \
                        -Dsonar.login=${SONAR_TOKEN} \
                        -Dsonar.host.url=http://192.168.1.102:9000
                    """
                }
            }
        }

    }
}