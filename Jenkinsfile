pipeline {
    agent any
    environment {
        PROJECT_ID = 'bilvantisaimlproject'
    }
    stages {
        stage('Authenticate with GCP') {
            steps {
                withCredentials([file(credentialsId: 'wif-config-file', variable: 'WIF_CONFIG')]) {
                    script {
                        // Copy the credential file to a known location
                        sh 'mkdir -p $WORKSPACE/temp'
                        sh 'cp $WIF_CONFIG $WORKSPACE/temp/wif-config.json'
                        
                        // Authenticate
                        sh """
                            gcloud auth login --cred-file=$WORKSPACE/temp/wif-config.json
                            export GOOGLE_APPLICATION_CREDENTIALS=$WORKSPACE/temp/wif-config.json
                        """
                    }
                }
            }
        }
        stage('List GCP Resources') {
            steps {
                sh """
                    gcloud storage buckets list --project=${PROJECT_ID}
                    // gcloud compute instances list --project=${PROJECT_ID}
                """
            }
        }
    }
}
