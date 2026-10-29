
pipeline {
    agent any

    environment {

DOCKER = 'C:\\Users\\tawfi\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'

    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate') {
            steps {
                bat 'if not exist site\\index.html exit /b 1'
                bat 'if not exist Dockerfile exit /b 1'
                echo 'Project files validated successfully'
            }
        }

        stage('Check Docker') {
            steps {
                bat '"%DOCKER%" --version'
                bat '"%DOCKER%" info'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '"%DOCKER%" build -t kanban-workflow .'
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                    "%DOCKER%" rm -f kanban-workflow-app
                    "%DOCKER%" run -d --name kanban-workflow-app -p 8081:80 kanban-workflow
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded. Open http://localhost:8081'
        }
        failure {
            echo 'Pipeline failed. Review Console Output.'
        }
    }
}
