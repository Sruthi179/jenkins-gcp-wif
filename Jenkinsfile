pipeline {
    agent {
        label 'ubuntu-latest'
    }
    
    environment {
        PROJECT_ID = 'bilvantisaimlproject'
        WIF_PROVIDER = 'projects/286895835019/locations/global/workloadIdentityPools/jenkins-pool-v2/providers/jenkins-provider-v2'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Google Cloud SDK') {
            steps {
                sh '''
                    if ! command -v gcloud &> /dev/null; then
                        echo "Installing Google Cloud SDK..."
                        curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-latest-linux-x86_64.tar.gz
                        tar -xf google-cloud-sdk-latest-linux-x86_64.tar.gz
                        ./google-cloud-sdk/install.sh --quiet
                        export PATH=$PATH:$PWD/google-cloud-sdk/bin
                    else
                        echo "Google Cloud SDK already installed"
                    fi
                '''
            }
        }
        
        stage('Configure Workload Identity Federation') {
            steps {
                withCredentials([string(credentialsId: 'wif-config-file', variable: 'WIF_TOKEN')]) {
                    sh '''
                        # Generate OIDC token file
                        echo $WIF_TOKEN > /tmp/oidc_token.txt
                        
                        # Authenticate using Workload Identity Federation
                        gcloud iam workload-identity-pools create-cred-config ${WIF_PROVIDER} \
                            --service-account="jenkins-wif-sa-v2@bilvantisaimlproject.iam.gserviceaccount.com" \
                            --output-file=/tmp/credentials.json \
                            --credential-source-file=/tmp/oidc_token.txt
                        
                        # Authenticate with the generated config
                        gcloud auth login --cred-file=/tmp/credentials.json
                        
                        # Set project
                        gcloud config set project ${PROJECT_ID}
                    '''
                }
            }
        }
        
        stage('List Google Cloud Storage Buckets') {
            steps {
                sh 'gcloud storage buckets list | grep "name"'
            }
        }
    }
    
    post {
        always {
            sh 'rm -f /tmp/oidc_token.txt /tmp/credentials.json || true'
            cleanWs()
        }
    }
}
