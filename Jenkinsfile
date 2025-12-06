pipeline {
    agent any  // just use the Jenkins container itself

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to Firebase Hosting') {
            steps {
                sh 'firebase deploy --only hosting'
            }
        }
    }
}
