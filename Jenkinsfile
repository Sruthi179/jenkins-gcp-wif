pipeline {
    agent any
    environment {
        GCP_PROJECT = 'bilvantisaimlproject'
    }
    stages {
        stage('Authenticate with GCP') {
            steps {
                withCredentials([file(credentialsId: 'wif-config-file', variable: 'WIF_CONFIG')]) {
                    script {
                        // Create workspace directory
                        sh 'mkdir -p $WORKSPACE/config'
                        // Copy credential file
                        sh 'cp "$WIF_CONFIG" "$WORKSPACE/config/wif.json"'
                        // Authenticate using WIF
                        sh """
                            gcloud auth login --cred-file="$WORKSPACE/config/wif.json"
                            gcloud config set project $GCP_PROJECT
                            export GOOGLE_APPLICATION_CREDENTIALS="$WORKSPACE/config/wif.json"
                        """
                    }
                }
            }
        }
        stage('List GCP Resources') {
            steps {
                sh """
                    gcloud storage buckets list --project=$GCP_PROJECT
                    gcloud compute instances list --project=$GCP_PROJECT
                """
            }
        }
    }
}
