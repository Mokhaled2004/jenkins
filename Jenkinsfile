pipeline {
    agent {
        docker {
            image 'node:18' // has npm + npx
            args '-u root:root'
        }
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

        stage('Deploy to Firebase Hosting') {
            steps {
                sh 'npx firebase-tools deploy --only hosting'
            }
        }
    }
}
