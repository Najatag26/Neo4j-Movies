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
                withCredentials([string(credentialsId: 'SonarQubeToken', variable: 'SONAR_AUTH_TOKEN')]) {
                    script {
                        def scannerHome = tool 'SonarQube Scanner'
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=Automatisation_Project \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=${env.SONAR_AUTH_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Build') {
            steps {
                def mvnHome = tool name: 'Maven', type: 'maven'
                sh "${mvnHome}/bin/mvn clean install"
            }
        }

        stage('Tests') {
            steps {
                def mvnHome = tool name: 'Maven', type: 'maven'
                sh "${mvnHome}/bin/mvn test"
            }
        }
    }
}
