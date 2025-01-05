pipeline {
    agent any

    environment {
        
        SONAR_TOKEN = credentials('sonar-token')
    }

    tools {
       
        maven 'Maven'
    }

    stages {
        stage('Cloner le projet') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/Najatag26/Neo4j-Movies.git' // URL du dépôt
            }
        }

        stage('Analyse SonarQube') {
            steps {
                
                withSonarQubeEnv('SonarQube-Server') {
                    sh "mvn clean verify sonar:sonar -Dsonar.projectKey=Neo4j-Movies -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }
    }
}
