pipeline{
    agent any
    environment{
        COMPOSE_FILE = 'docker-compose.yml'
    }
    stages{
        stage('checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/chaitanya461/docker-compose-project.git'
            }
        }
        stage('Creatingcontainer'){
            steps{
                sh '''
                     docker compose down || true
                     docker compose up -d
                     '''
            }
        }
        stage('verify'){
            steps{
                sh 'docker ps'
            }
        }
    }
}
