pipeline {
    agent {
        docker {
            image 'node:18'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/NoaMatout/chuck_citations-front', branch: 'main'
                sh 'ls -la'
            }
        }

        stage('Test') {
            steps {
                sh '''
                npm install
                npm run test
                '''
            }
        }

        stage('Build and push image') {
            steps {
                sh '''
                docker build -t registry.home.arpa:5000/chuck_front:latest .
                docker push registry.home.arpa:5000/chuck_front:latest
                '''
            }
        }
    }
}
