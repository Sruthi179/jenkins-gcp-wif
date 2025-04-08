pipeline {
    agent any

    stages {
        stage("Generating OIDC token and saving it to a file") {
            steps {
                script {
                    sh '''
                        mkdir -p ${WORKSPACE}/token
                        gcloud auth print-identity-token 286895835019-compute@developer.gserviceaccount.com \
                          --audiences="//iam.googleapis.com/projects/286895835019/locations/global/workloadIdentityPools/jenkins-pool-v2/providers/jenkins-provider-v2" \
                          > ${WORKSPACE}/token/key
                    '''
                }
            }
        }

        stage("Getting the WIF config file into a variable") {
            steps {
                withCredentials([file(credentialsId: 'wif-config-file', variable: 'WIF')]) {
                    sh '''
                        export GOOGLE_EXTERNAL_ACCOUNT_FILE=$WIF
                        export GOOGLE_EXTERNAL_ACCOUNT_TOKEN_FILE=${WORKSPACE}/token/key

                        # Debug - check if token file exists
                        echo "Token file path: $GOOGLE_EXTERNAL_ACCOUNT_TOKEN_FILE"
                        ls -l $GOOGLE_EXTERNAL_ACCOUNT_TOKEN_FILE

                        # Activate credentials using WIF and token
                        env \
                          GOOGLE_EXTERNAL_ACCOUNT_FILE=$GOOGLE_EXTERNAL_ACCOUNT_FILE \
                          GOOGLE_EXTERNAL_ACCOUNT_TOKEN_FILE=$GOOGLE_EXTERNAL_ACCOUNT_TOKEN_FILE \
                          gcloud auth login --brief --cred-file=$GOOGLE_EXTERNAL_ACCOUNT_FILE --quiet

                        # Confirm auth works
                        gcloud container clusters list --project=bilvantisaimlproject
                        gcloud compute instances list --project=bilvantisaimlproject
                    '''
                }
            }
        }
    }
}
