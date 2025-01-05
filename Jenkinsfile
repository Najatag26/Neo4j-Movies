pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'najatag/neo4j-movies:latest'
        DOCKER_USER = 'najatag' 
        DOCKER_PASS = credentials('dockerhub-credential') // Credentials Docker Hub
        SONAR_TOKEN = credentials('sonar-token') // Token SonarQube
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

        stage('Quality Gate') {
            steps {
                script {
                    // Vérifier l'état du Quality Gate
                    def qualityGate = waitForQualityGate()
                    if (qualityGate.status != 'OK') {
                        error "Quality Gate failed: ${qualityGate.status}"
                    }
                }
            }
        }

        stage('Build Project') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Construire l'image Docker
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credential', 
                                                     usernameVariable: 'DOCKER_USER', 
                                                     passwordVariable: 'DOCKER_PASS')]) {
                        // Se connecter à Docker Hub
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        // Pousser l'image Docker
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }
    }
}
