pipeline {
    agent any

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')
    }

    options {
        timestamps()
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Starting Git checkout…"
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [
                        [$class: 'CleanBeforeCheckout'],
                        [$class: 'PruneStaleBranch'],
                        [$class: 'WipeWorkspace']
                    ],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Mokhaled2004/JENKINS.git'
                    ]]
                ])
                echo "Checkout completed successfully!"
            }
        }

        stage('List Workspace') {
            steps {
                echo "Listing workspace files…"
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('Deploy to Firebase') {
            steps {
                echo "Deploying to Firebase hosting..."
                dir('firebase-app') {
                    // ✅ Firebase CLI is already installed globally as root
                    sh 'firebase --version'
                    sh 'firebase deploy --only hosting --token "$FIREBASE_TOKEN" --debug'
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline finished successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
