pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000 -p 5000:5000 -u 0'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deploy to Staging') {
            when {
                branch 'master' 
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy for production') {
            when {
                buildingTag()
            }
            steps {
                input message: 'Would you like to deploy to production'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
