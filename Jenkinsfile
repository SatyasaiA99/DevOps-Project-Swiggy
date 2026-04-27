pipeline {
    agent any

    tools {
        nodejs 'node23'   // Make sure this exists in Jenkins tools
    }

    environment {
        APP_NAME = "swiggy-app"
        CONTAINER_NAME = "swiggy-container"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/SatyasaiA99/DevOps-Project-Swiggy.git', branch: 'master'
            }
        }

        stage('Check Node Version') {
            steps {
                sh 'node -v'
                sh 'npm -v'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $APP_NAME .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name $CONTAINER_NAME $APP_NAME'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
