pipeline {
    agent {
        docker {
            image 'node:18'       // Node.js + npm included
            args '-u root:root'    // run as root inside container
        }
    }

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')  // Jenkins secret
    }

    options {
        skipDefaultCheckout()    // we’ll do explicit checkout
        timestamps()             // adds timestamps to logs
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()  // remove old files to avoid stale deploys
            }
        }

        stage('Checkout Repo') {
            steps {
                checkout scm  // fetch the repo from GitHub
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
                dir("${WORKSPACE}") {  // make sure we’re at workspace root
                    sh '''
                    echo "Deploying to Firebase..."
                    npx firebase deploy --only hosting --token $FIREBASE_TOKEN --force --debug
                    '''
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
