pipeline {
    agent any
    environment {
        GOOGLE_APPLICATION_CREDENTIALS = credentials('WIF_CONFIG')
    }
    stages {
        stage('Authenticate with GCP') {
            steps {
                sh '''
                    echo "Authenticating with Workload Identity Federation..."
                    gcloud auth application-default print-access-token
                '''
            }
        }

        stage('List GCP Resources') {
            steps {
                sh 'gcloud projects list'
            }
        }
    }
}
