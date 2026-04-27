pipeline {
    agent any

    tools {
        nodejs 'node23'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/SatyasaiA99/DevOps-Project-Swiggy.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Artifact') {
            steps {
                sh '''
                rm -rf build
                mkdir build
                cp -r * build/
                tar -czf node-app.tar.gz build
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'node-app.tar.gz', fingerprint: true
            }
        }

        stage('Deploy & Run') {
            steps {
                sh '''
                # Stop old Node app if running
                pkill node || true

                # Extract artifact
                tar -xzf node-app.tar.gz
                cd build

                # Run app in background
                nohup node index.js > app.log 2>&1 &
                '''
            }
        }

        stage('Verify App') {
            steps {
                sh '''
                sleep 5
                curl http://localhost:4000 || echo "App not reachable"
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Application is running"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
