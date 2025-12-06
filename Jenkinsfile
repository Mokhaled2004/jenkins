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

    options {
        skipDefaultCheckout()
        timestamps()
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout Repo') {
            steps {
                checkout scm
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
                dir("${WORKSPACE}") {
                    sh 'npx firebase deploy --only hosting --force --token $FIREBASE_TOKEN --debug'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed. Check logs!"
        }
    }
}
