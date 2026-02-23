pipeline {
    agent any

    stages {

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

        stage('Package') {
            steps {
                sh 'tar -czf react-demo-app.tar.gz build'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '*.tar.gz', fingerprint: true
            }
        }

    }
}
