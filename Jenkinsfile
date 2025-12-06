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
                sh 'git clone https://github.com/Mokhaled2004/jenkins.git .'
            }
        }

        stage('Install Firebase Tools') {
            steps {
                sh '''
                if ! command -v firebase &> /dev/null
                then
                    echo "Installing firebase-tools..."
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
