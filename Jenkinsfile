pipeline {
    agent none // we’ll specify agent per stage

    options {
        skipStagesAfterUnstable()
    }

    stages {
        stage('Build & Test') {
            agent {
                docker {
                    image 'maven:3.9.9-eclipse-temurin-17'
                    label 'k8s-agent' // Kubernetes pod label
                }
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
            }
        }

        stage('Docker Build') {
            agent { label 'docker-node' } // runs on the node with Docker installed
            steps {
                // Copy the built jar from workspace (Maven pod workspace is persisted)
                sh 'docker build -t springio/gs-spring-boot-docker .'
            }
        }
    }
}