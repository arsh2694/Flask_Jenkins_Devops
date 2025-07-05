pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flaskapp-autodeployer:latest'
        CONTAINER_NAME = 'flaskapp'
        APP_PORT = '5000'
    }

    stages {

        stage('📥 Clone Repository') {
            steps {
               git 'https://github.com/dilip8700/flask-jenkins-devops.git'
            }
        }

        stage('📦 Install Python Dependencies') {
            steps {
                sh 'sudo pip install -r requirements.txt'
            }
        }

        stage('✅ Run Unit Tests') {
            steps {
                sh 'sudo pytest'
            }
        }

        stage('🐳 Build Docker Image') {
            steps {
                sh "sudo docker build -t $IMAGE_NAME ."
            }
        }

        stage('🧹 Clean Existing Container') {
            steps {
                sh """
                    sudo docker stop $CONTAINER_NAME || true
                    sudo docker rm $CONTAINER_NAME || true
                """
            }
        }

        stage('🚀 Run New Container') {
            steps {
                sh "sudo docker run -d -p $APP_PORT:$APP_PORT --name $CONTAINER_NAME $IMAGE_NAME"
            }
        }
    }

    post {
        success {
            echo '✅ Flask App Deployed Locally via Docker!'
        }
        failure {
            echo '❌ Build/Deployment Failed. Check Console Output.'
        }
    }
}
