pipeline {
    agent any
    environment {
        DOCKER_HOST = "tcp://docker:2375"   // talk to DinD service
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker run --rm -v $PWD:/app -w /app maven:3.9.6-eclipse-temurin-17 mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --rm -v $PWD:/app -w /app maven:3.9.6-eclipse-temurin-17 mvn test'
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
                echo 'Job Complete!'
            }
        }
    }
}
