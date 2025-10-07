pipeline {
    agent any

    tools {
        nodejs "NodeJS"   // must match the name you gave in Jenkins Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/prudhvivarma96/nodejs-jenkins-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Run App') {
            steps {
                sh 'nohup npm start &'
            }
        }
    }
}

