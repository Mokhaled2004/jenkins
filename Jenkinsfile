pipeline {
    agent any

    tools {
        nodejs "node25" // ← Must match the name you chose in Jenkins
    }

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to Firebase Hosting') {
            steps {
                sh 'npx firebase-tools deploy --only hosting'
            }
        }
    }
}
