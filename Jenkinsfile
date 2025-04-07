pipeline {
    agent any

    environment {
        PROJECT_ID      = 'bilvantisaimlproject'
        SERVICE_ACCOUNT = "jenkins-gcp-sa@bilvantisaimlproject.iam.gserviceaccount.com"
        WIF_PROVIDER    = "projects/286895835019/locations/global/workloadIdentityPools/jenkins-pool/providers/jenkins-provider"
    }

    stages {
        stage('Authenticate with GCP') {
            steps {
                withCredentials([file(credentialsId: 'wif-config-file', variable: 'WIF_CONFIG')]) {
                    sh '''
                        echo "Setting GOOGLE_APPLICATION_CREDENTIALS to federated config..."
                        export GOOGLE_APPLICATION_CREDENTIALS=$WIF_CONFIG

                        echo "Verifying access token retrieval..."
                        ACCESS_TOKEN=$(gcloud auth application-default print-access-token)
                        if [ -z "$ACCESS_TOKEN" ]; then
                          echo "Failed to retrieve access token."
                          exit 1
                        else
                          echo "Access token retrieved successfully."
                        fi
                    '''
                }
            }
        }

        stage('List GCP Resources') {
            steps {
                sh '''
                    echo "Listing GCP Buckets and Compute Instances..."
                    gcloud storage buckets list --project=$PROJECT_ID
                    gcloud compute instances list --project=$PROJECT_ID
                '''
            }
        }
    }
}
