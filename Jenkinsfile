pipeline {
    agent any

    gent any
    environment {
        DOCKER_IMAGE = 'neo4j-movies:latest' // Nom de l'image Docker
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

stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker tag ${DOCKER_IMAGE} ${DOCKER_USER}/${DOCKER_IMAGE}"
                        sh "docker push ${DOCKER_USER}/${DOCKER_IMAGE}"
                    }
                }
            }
        }
