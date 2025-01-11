pipeline{
    agent any

    tools {
        jdk 'java-11'
        maven 'maven'
    }

    stages{
        stage('Git-Checkout'){
            steps{
                git branch: 'master', url: 'https://github.com/manjukolkar/web-application.git'
            }
        }
        stage('Compile'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('Build'){
            steps{
                sh 'mvn package'
            }
        }
    }
}
