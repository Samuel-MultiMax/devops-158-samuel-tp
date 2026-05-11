pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/Samuel-MultiMax/devops-158-samuel-tp'
            }
        }

        stage('Pull latest code') {
            steps {
                dir('/var/snap/jenkins/common/devops-158-samuel-tp') {
                    git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/Samuel-MultiMax/devops-158-samuel-tp'
                }
            }
        }

        stage('Install dependencies') {
            steps {
                dir('/var/snap/jenkins/common/devops-158-samuel-tp') {
                    sh '''
                        . venv/bin/activate
                        pip install flask
                    '''
                }
            }
        }

        stage('Run unit tests') {
            steps {
                dir('/var/snap/jenkins/common/devops-158-samuel-tp') {
                    sh '''
                        . venv/bin/activate
                        python -m pytest test_app.py -v --tb=short
                    '''
                }
            }
            post {
                success {
                    echo 'Tous les tests unitaires sont passés avec succès !'
                }
                failure {
                    echo 'Échec des tests unitaires. Le déploiement est annulé.'
                }
            }
        }

        stage('Restart Flask app') {
            steps {
                script {
                    sh 'sudo systemctl restart flask-app'
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
