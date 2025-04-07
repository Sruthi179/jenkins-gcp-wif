pipeline {
    agent any
    environment {
        PROJECT_ID      = 'bilvantisaimlproject'
        SERVICE_ACCOUNT = "jenkins-gcp-sa@bilvantisaimlproject.iam.gserviceaccount.com"
        WIF_PROVIDER    = 'projects/286895835019/locations/global/workloadIdentityPools/jenkins-pool/providers/jenkins-provider'
    }
    stages {
        stage('Authenticate with GCP') {
            steps {
                withCredentials([file(credentialsId: 'wif-config-file', variable: 'WIF_CONFIG')]) {
                    sh """
                        export GOOGLE_APPLICATION_CREDENTIALS=${WIF_CONFIG}
                        gcloud auth login --brief --cred-file=${WIF_CONFIG}
                    """
                }
            }
        }
        stage('List GCP Resources') {
            steps {
                withCredentials([file(credentialsId: 'wif-config-file', variable: 'WIF_CONFIG')]) {
                    sh """
                        export GOOGLE_APPLICATION_CREDENTIALS=${WIF_CONFIG}
                        gcloud storage buckets list --project=${PROJECT_ID}
                        gcloud compute instances list --project=${PROJECT_ID}
                    """
                }
            }
        }
    }
}
