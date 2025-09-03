pipeline {
    agent any

    environment {
        // Point Jenkins pipelines to Docker-in-Docker service
        DOCKER_HOST = "tcp://docker:2375"
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    docker run --rm \
                      -u $(id -u):$(id -g) \
                      -v $PWD:/app \
                      -w /app \
                      maven:3.9.6-eclipse-temurin-17 \
                      mvn -B -DskipTests clean package
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    docker run --rm \
                      -u $(id -u):$(id -g) \
                      -v $PWD:/app \
                      -w /app \
                      maven:3.9.6-eclipse-temurin-17 \
                      mvn test
                '''
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }

        stage('Complete') {
            steps {
                echo '✅ Pipeline complete!'
            }
        }
    }
}
