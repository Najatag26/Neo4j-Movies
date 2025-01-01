pipeline {
    agent any

    tools {
        maven 'Maven' // Assurez-vous que "Maven" est configuré dans "Manage Jenkins > Global Tool Configuration"
    }

    stages {
        stage('Cloner le dépôt Git') {
            steps {
                git branch: 'main', credentialsId: '19dfc1cb-3cab-444f-828c-ba259cdcff03', url: 'git@github.com:Najatag26/Automatisation_Project.git'
            }
        }

        stage('Analyse SonarQube') {
            steps {
                withCredentials([string(credentialsId: 'Jenkinsfile', variable: 'SONAR_AUTH_TOKEN')]) {
                    script {
                        def scannerHome = tool 'Jenkinsfile' // Assurez-vous que "SonarQube Scanner" est configuré dans Jenkins
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=Automatisation_Project \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://sonarqube:9000 \
                            -Dsonar.login=${env.SONAR_AUTH_TOKEN}
                            -X
                        """
                    }
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
