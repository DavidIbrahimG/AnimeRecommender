pipeline{
    agent any

    environment {
        VENV_DIR = 'venv'
        GCP_PROJECT = 'flash-district-467713-g8'
        GCLOUD_PATH = "/var/jenkins_home/google-cloud-sdk/bin"
        KUBECTL_AUTH_PLUGIN = "/usr/lib/google-cloud-sdk/bin"
    }


    stages {
        stage('Cloning from Github ....') {
            steps {
                script {
                    echo 'Cloning the repository from Github'
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-token', url: 'https://github.com/DavidIbrahimG/AnimeRecommender.git']])
                }
            }
        }

        stage('Creating a virtual environment ....') {
            steps {
                script {
                    echo 'Creating a virtual environment'
                    sh '''
                    python -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    pip install --upgrade pip
                    pip install -e .
                    pip install  dvc
                    '''
                }
            }
        }

        stage('DVC Pull'){
            steps{
                withCredentials([file(credentialsId:'gcp-key' , variable: 'GOOGLE_APPLICATION_CREDENTIALS' )]){
                    script{
                        echo 'DVC Pull....'
                        sh '''
                        . ${VENV_DIR}/bin/activate
                        dvc pull
                        '''
                    }
                }
            }
        }

        
        
    }
}