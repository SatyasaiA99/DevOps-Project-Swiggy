pipeline {
    agent any

    tools {
        nodejs 'node18'
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
                rm -rf build node-app.tar.gz
                mkdir build

                cp -r package*.json build/
                cp -r *.js build/ || true
                cp -r config build/ || true
                cp -r routes build/ || true
                cp -r controllers build/ || true
                cp -r models build/ || true

                cd build
                npm install --production
                cd ..

                tar -czf node-app.tar.gz build
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'node-app.tar.gz', fingerprint: true
            }
        }

        stage('Run App (Temporary)') {
            steps {
                sh '''
                pkill node || true
                tar -xzf node-app.tar.gz
                cd build
                nohup node index.js > app.log 2>&1 &
                '''
            }
        }

        stage('Check Public Access') {
            steps {
                sh '''
                sleep 5

                echo "Checking via localhost..."
                curl http://localhost:4000 || echo "Local check failed"

                echo "Checking via Public IP..."
                curl http://$(curl -s ifconfig.me):4000 || echo "Public IP check failed"
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Artifact created & app accessible via public IP"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
