pipeline {
    agent {label 'local'}

    environment {
        DOCKER_HUB_USERNAME = 'presedinte'
        DOCKER_HUB_REPO = 'petclinic-jenkins-java-workshop'
    }

    tools {
        maven 'M3'
        jdk 'ZuluJDK 8'
    }

    stages {
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def tag = env.BUILD_NUMBER
                    sh "docker build -t ${env.DOCKER_HUB_USERNAME}/${env.DOCKER_HUB_REPO}:${tag} ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        def tag = env.BUILD_NUMBER
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh "docker push ${env.DOCKER_HUB_USERNAME}/${env.DOCKER_HUB_REPO}:${tag}"
                    }
                }
            }
        }
    }
}
