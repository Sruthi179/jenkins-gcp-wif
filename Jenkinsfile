pipeline {
    agent any

    environment {
        PROJECT_ID      = 'bilvantisaimlproject' // Replace with your GCP project ID
        SERVICE_ACCOUNT = "jenkins-gcp-sa@bilvantisaimlproject.iam.gserviceaccount.com"
        WIF_PROVIDER    = "projects/286895835019/locations/global/workloadIdentityPools/jenkins-pool/providers/jenkins-provider"
    }

    stages {
        stage('Authenticate with GCP') {
            steps {
                withCredentials([file(credentialsId: 'wif-config-file', variable: 'WIF_CONFIG')]) {
                    sh '''
                        echo "Authenticating with Workload Identity Federation..."
                        export GOOGLE_APPLICATION_CREDENTIALS=$WIF_CONFIG
                        gcloud auth application-default login --cred-file=$GOOGLE_APPLICATION_CREDENTIALS
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
