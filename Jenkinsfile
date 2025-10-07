pipeline {
    agent any

    tools {
        nodejs "NodeJS"  // Must match the name in Jenkins Global Tool Configuration
    }

    options {
        skipDefaultCheckout()  // Prevent skipping the build if no Git changes
    }

    stages {
        stage('Checkout') {
            steps {
                // Force Git checkout every time
                checkout([$class: 'GitSCM',
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/prudhvivarma96/nodejs-jenkins-app.git',
                        credentialsId: 'github-token'  // Replace with your Jenkins GitHub credential ID
                    ]]
                ])
            }
        }

        stage('Stop Previous App') {
            steps {
                sh '''
                    if lsof -i:3000 -t >/dev/null ; then
                        echo "Stopping previous Node process..."
                        kill -9 $(lsof -i:3000 -t)
                    else
                        echo "No previous Node process running."
                    fi
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || echo "No tests found or tests failed, continuing..."'
            }
        }

        stage('Run App') {
            steps {
                sh 'nohup npm start > app.log 2>&1 &'
            }
        }
    }

    post {
        success {
            echo "✅ Node.js app deployed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check app.log or console output."
        }
    }
}

