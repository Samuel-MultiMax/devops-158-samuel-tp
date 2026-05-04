pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-credentials',  url: 'https://github.com/Samuel-MultiMax/devops-158-samuel-tp'
            }
        }

        stage('Pull latest code') {
            steps {
                dir('/home/samuel/devops-158-samuel-tp') {
                    git branch: 'main', credentialsId: 'github-credentials',  url: 'https://github.com/Samuel-MultiMax/devops-158-samuel-tp'
                }
            }
        }

        stage('Install dependencies') {
            steps {
                dir('/home/samuel/devops-158-samuel-tp') {
                    sh '''
                        source venv/bin/activate
                        pip install flask
                    '''
                }
            }
        }

        stage('Restart Flask app') {
            steps {
                script {
                    sh 'pkill -f "python app.py" || true'
                    sh '''
                        cd /home/samuel/devops-158-samuel-tp
                        source venv/bin/activate
                        nohup python app.py > flask.log 2>&1 &
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Déploiement automatique réussi ! BRAVO DAMN'
        }
        failure {
            echo 'Échec du pipeline. - AIE AIE AIE CA PUE'
        }
    }
}
