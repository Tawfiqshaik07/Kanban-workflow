
pipeline {
    agent any

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

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t kanban-workflow .'
            }
        }

        stage('Deploy') {
            steps {
                
bat '''
    docker rm -f kanban-workflow-app >NUL 2>&1
    docker run -d --name kanban-workflow-app -p 8081:80 kanban-workflow
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
