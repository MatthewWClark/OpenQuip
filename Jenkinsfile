pipeline {
    agent any
    tools {
        maven 'Maven-3.9.9' // matches name in Global Tool Config
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
               sh "docker build -t springio/gs-spring-boot-docker:${env.BUILD_NUMBER} ."
            }
        }
    }
}