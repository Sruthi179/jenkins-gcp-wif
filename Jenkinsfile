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
                        // Authenticate and persist
                        sh """
                            gcloud auth activate-service-account --key-file="$WORKSPACE/config/wif.json"
                            gcloud config set project $GCP_PROJECT
                            # Store credentials in gcloud's default location
                            cp "$WORKSPACE/config/wif.json" "$HOME/gcp_credentials.json"
                            export GOOGLE_APPLICATION_CREDENTIALS="$WORKSPACE/config/wif.json"
                        """
                    }
                }
            }
        }
        stage('List GCP Resources') {
            steps {
                sh """
                    # Explicitly use the credentials
                    gcloud auth activate-service-account --key-file="$WORKSPACE/config/wif.json"
                    gcloud storage buckets list --project=$GCP_PROJECT
                    gcloud compute instances list --project=$GCP_PROJECT
                """
            }
        }
    }
}
