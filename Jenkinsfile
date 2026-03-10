pipeline {
        agent {
            docker {
                image 'jenkins/jnlp-agent-maven:windows-nanoserver-jdk11' // Example image from Docker Hub
                label 'docker-agent'
            }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t springio/gs-spring-boot-docker .'
            }
        }
    }
}
