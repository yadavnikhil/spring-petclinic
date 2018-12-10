#!/usr/bin/env groovy

pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /home/jenkins/.m2:/root/.m2 -v /var/run/docker.sock:/var/run/docker.sock:rw'
        }
    }

    stages {
        stage('Build Maven Artifacts') {
            steps {
                sh 'mvn -DskipTests clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'mvn docker:build'
            }
        }

        stage('Test Docker Image') {
            steps {
                sh 'mvn docker:start'
            }
            post {
                always {
                    sh 'mvn docker:stop'
                }
            }
        }

        stage('Publish DOcker Image') {
            steps {
                sh 'mvn docker:push'
            }
        }
    }
}
