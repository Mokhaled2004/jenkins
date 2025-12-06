pipeline {
    agent {
        docker {
            image 'node:18'
            args '-u root:root'
        }
    }

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                // Properly checkout your repo
                checkout scm
            }
        }

        stage('Install Firebase Tools') {
            steps {
                // Install only if not already installed
                sh '''
                if ! command -v firebase &> /dev/null
                then
                    npm install -g firebase-tools
                else
                    echo "firebase-tools already installed"
                fi
                '''
            }
        }

        stage('Deploy to Firebase Hosting') {
            steps {
                sh 'npx firebase deploy --only hosting'
            }
        }
    }
}
