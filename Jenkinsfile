pipeline {
    agent any
    environment {
                DOCKERHUB_CREDENTIALS = credentials('jk-dh-tk')
    }

    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'main', credentialsId: 'jk-gh-tk', url: 'https://github.com/chmviola/jk-private-gh.git'
            }
        }
        
        stage('Building Image') {
            steps {
                sh 'docker build -t chmviola/webapp:${BUILD_NUMBER} .'
            }
        }
        
        stage('Login DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push Image') {
            steps {
                sh 'docker push chmviola/webapp:$BUILD_NUMBER'
            }
        }
        
        stage('Deployment Image') {
            steps {
                sh 'docker run --rm -d -p 3000:3000 --name webapp_ctr chmviola/webapp:${BUILD_NUMBER}'
            }
        }
    }
}
