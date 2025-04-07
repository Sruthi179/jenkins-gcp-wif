pipeline {
    agent any
    environment {
        GCP_PROJECT = 'bilvantisaimlproject'
        CREDS_DIR = "${WORKSPACE}/.gcp_creds"  # Use hidden directory in workspace
    }
    stages {
        stage('Prepare Environment') {
            steps {
                // Create directory with proper permissions
                sh """
                    mkdir -p "${CREDS_DIR}"
                    chmod 755 "${CREDS_DIR}"
                """
            }
        }
        stage('Authenticate with GCP') {
            steps {pipeline {
    agent any
    environment {
        PROJECT_ID = 'searce-playground-v1'
        CREDS_DIR = "${WORKSPACE}/.gcp_creds"
    }
    stages {
        stage('Prepare Environment') {
            steps {
                sh "mkdir -p ${CREDS_DIR}"
            }
        }
        stage('Authenticate') {
            steps {
                withCredentials([file(credentialsId: 'wif-config-file', variable: 'WIF')]) {
                    sh """
                        cp "$WIF" "${CREDS_DIR}/wif.json"
                        gcloud auth login --cred-file="${CREDS_DIR}/wif.json"
                        gcloud config set project ${PROJECT_ID}
                    """
                }
            }
        }
        stage('List Resources') {
            steps {
                sh """
                    gcloud container clusters list
                    gcloud compute instances list
                """
            }
        }
    }
}
                withCredentials([file(credentialsId: 'wif-config-file', variable: 'WIF_CONFIG')]) {
                    script {
                        // Copy with explicit permissions
                        sh """
                            cp "${WIF_CONFIG}" "${CREDS_DIR}/wif.json"
                            chmod 644 "${CREDS_DIR}/wif.json"
                        """
                        // Authenticate
                        sh """
                            gcloud auth login --cred-file="${CREDS_DIR}/wif.json" --quiet
                            gcloud config set project ${GCP_PROJECT}
                            export GOOGLE_APPLICATION_CREDENTIALS="${CREDS_DIR}/wif.json"
                        """
                    }
                }
            }
        }
        stage('List GCP Resources') {
            steps {
                sh """
                    gcloud storage buckets list --project=${GCP_PROJECT}
                    gcloud compute instances list --project=${GCP_PROJECT}
                """
            }
        }
    }
    post {
        always {
            // Clean up credentials
            sh "rm -rf ${CREDS_DIR}"
        }
    }
}
