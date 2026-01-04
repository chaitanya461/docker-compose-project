sudo usermod -aG docker jenkins
sudo systemctl restart jenkins

----------------
                use these update version

sudo mkdir -p /usr/lib/docker/cli-plugins
sudo curl -SL https://github.com/docker/compose/releases/download/v2.25.0/docker-compose-linux-x86_64 \
  -o /usr/lib/docker/cli-plugins/docker-compose
sudo chmod +x /usr/lib/docker/cli-plugins/docker-compose

---------------

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
