pipeline {
    agent any
    environment {
        DOCKER_HOST = "tcp://docker:2375"
    }
    stages {
        stage('Debug Files') {
            steps {
                sh """
                    echo "Workspace: ${env.WORKSPACE}"
                    docker run --rm -v ${env.WORKSPACE}:/app -w /app maven:3.9.6-eclipse-temurin-17 sh -c "pwd && ls -la"
                """
            }
        }
        stage('Build') {
            steps {
                sh "docker run --rm -v ${env.WORKSPACE}:/app -w /app maven:3.9.6-eclipse-temurin-17 mvn clean package"
            }
        }
        stage('Test') {
            steps {
                sh "docker run --rm -v ${env.WORKSPACE}:/app -w /app maven:3.9.6-eclipse-temurin-17 mvn test"
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
