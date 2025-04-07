pipeline {
    agent any
    environment {
        PROJECT_ID      = 'bilvantisaimlproject'       // Replace with your GCP project ID
        SERVICE_ACCOUNT = 'jenkins-gcp-sa@${PROJECT_ID}.iam.gserviceaccount.com'
        WIF_PROVIDER    = 'projects/286895835019/locations/global/workloadIdentityPools/jenkins-pool/providers/jenkins-provider'
    }
    stages {
        stage('Authenticate with GCP') {
            steps {
                withCredentials([file(credentialsId: 'wif-config-file', variable: 'WIF_CONFIG')]) {
                    sh """
                        gcloud auth login --cred-file=${WIF_CONFIG}
                        export GOOGLE_APPLICATION_CREDENTIALS=${WIF_CONFIG}
                    """
                }
            }
        }
        stage('List GCP Resources') {
            steps {
                sh """
                    gcloud storage buckets list --project=${PROJECT_ID}
                    gcloud compute instances list --project=${PROJECT_ID}
                """
            }
        }
    }
}
