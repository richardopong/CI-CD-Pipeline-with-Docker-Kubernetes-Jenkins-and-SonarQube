pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "richiekwas/java-app:latest"
        SONARQUBE_SERVER = "http://sonarqube:9000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/richardopong/CI-CD-Pipeline-with-Docker-Kubernetes-Jenkins-and-SonarQube'
            }
        }

        stage('Build Java Application') {
            steps {
                script {
                    docker.image('openjdk:17').inside {
                        sh 'java -version'
                        sh './mvnw clean package -DskipTests || mvn clean package -DskipTests'
                    }
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    docker.image('openjdk:11').inside {
                        sh 'java -version'
                        sh './mvnw test || mvn test'
                    }
                }
            }
        }

        stage('Static Code Analysis') {
            steps {
                script {
                    docker.image('openjdk:8').inside {
                        sh 'java -version'
                        sh 'mvn sonar:sonar -Dsonar.host.url=$SONARQUBE_SERVER'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'docker login -u YOUR_DOCKERHUB_USERNAME -p _*Pia2KLQ.L.C?T'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}