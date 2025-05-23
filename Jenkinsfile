pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'development', url: 'https://github.com/Neila-Nor/uts-devops.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'python3 -m venv venv'
                sh './venv/bin/pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh './venv/bin/python -m pytest tests/'
            }
        }

        stage('Deploy') {
            when {
                branch 'development'
            }
            steps {
                withCredentials([string(credentialsId: 'HEROKU_API_KEY', variable: 'HEROKU_API_KEY')]) {
                    sh '''
                        curl https://cli-assets.heroku.com/install.sh | sh
                        heroku auth:token
                        echo $HEROKU_API_KEY | heroku auth:token
                        heroku git:remote -a Neila-Nor
                        git push https://heroku:$HEROKU_API_KEY@git.heroku.com/nama-aplikasi-heroku-kamu.git HEAD:main -f
                    '''
                }
            }
        }
    }
}
