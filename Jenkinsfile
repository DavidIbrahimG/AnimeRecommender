pipeline{
    agent any

    stages {
        stage('Cloning from Github ....') {
            steps {
                script {
                    echo 'Cloning the repository from Github'
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-token', url: 'https://github.com/DavidIbrahimG/AnimeRecommender.git']])
                }
            }
        }
        
    }
}