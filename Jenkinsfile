pipeline {
    agent any

    tools {
        jdk 'java11'
        maven 'maven'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/yashb1117/Project-3.git'
            }
        }

        stage('Code Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Code Package') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build and Tag Docker Image') {
            steps {
                sh 'docker build -t app2 .'
            }
        }

        stage('Containerisation') {
            steps {
                sh 'docker run -it -d --name c8 -p 9008:8080 app2'
            }
        }

        stage('Login to Docker Hub and Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubpass', usernameVariable: 'dockerhubuser')]) {
                    sh 'docker login -u $dockerhubuser -p $dockerhubpass'
                    sh 'docker tag app2 $dockerhubuser/app2:latest'
                    sh 'docker push $dockerhubuser/app2:latest'
                }
            }
        }
stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig(
                    caCertificate: '',
                    clusterName: '',
                    contextName: '',
                    credentialsId: 'kubernetes',
                    namespace: '',
                    restrictKubeConfigAccess: false,
                    serverUrl: ''
                ) {
                    sh 'kubectl delete pods --all'
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }
}
