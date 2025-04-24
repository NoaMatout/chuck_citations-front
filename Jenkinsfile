pipeline {
    agent {
        label 'dind'
    }

    environment {
        DEPLOY_USER = 'vagrant'
        DEPLOY_HOST = '192.168.56.152'
        DEPLOY_PATH = '/var/www/html'
        CREDENTIALS_ID = 'ssh-dind-prod'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/NoaMatout/chuck_citations-front', branch: 'main'
                sh 'ls -la'
            }
        }

        stage('Build frontend') {
            steps {
                sh '''
                apk add --no-cache nodejs npm
                npm install
                npm run build
                '''
            }
        }

        stage('Deploy to production') {
            steps {
                sshagent (credentials: [env.CREDENTIALS_ID]) {
                    sh '''
                    echo "📦 Création du dossier /var/www/html sur le serveur prod"
                    ssh vagrant@192.168.56.152 'sudo mkdir -p /var/www/html && sudo chown -R vagrant:vagrant /var/www/html'
                    echo "🔄 Déploiement des fichiers dist/ vers ${DEPLOY_USER}@${DEPLOY_HOST}:${DEPLOY_PATH}"
                    scp -r dist/* ${DEPLOY_USER}@${DEPLOY_HOST}:${DEPLOY_PATH}
                    '''
                }
            }
        }
    }
}
