pipeline {
    agent any

    options {
        skipStagesAfterUnstable()
    }
    tools {
        maven 'Maven-3.9.9'
    }
    stages {
        stage('Build') {
            steps {
                sh '/usr/bin/mvn -B -DskipTests clean package'
            }
        }

        stage('Test') {
            steps {
                sh '/usr/bin/mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Docker Build') {
            steps {
                // This will run on the same node that has Docker installed
                sh 'docker build -t springio/gs-spring-boot-docker .'
            }
        }
    }
}