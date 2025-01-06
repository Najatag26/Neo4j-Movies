pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'neo4j-movies:latest' 
    }
    tools {
        maven 'Maven' 
    }
    tools{
        Docker 'Docker'
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
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
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
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credential', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker tag ${DOCKER_IMAGE} ${DOCKER_USER}/${DOCKER_IMAGE}"
                        sh "docker push ${DOCKER_USER}/${DOCKER_IMAGE}"
                    }
                }
            }
        }
       stage('Deploy and Start the Dockerized Project') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credential', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "docker pull ${DOCKER_USER}/${DOCKER_IMAGE}"
                        sh """
                        if [ \$(docker ps -aq -f name=neo4j-movies ]; then
                            docker rm -f neo4j-movies
                        fi
                        """
                        sh "docker run -d --name neo4j-movies -p 8082:8081 ${DOCKER_USER}/${DOCKER_IMAGE}"
                    }
                }
            }
        }
    } 
} 
