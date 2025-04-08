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

                        gcloud auth application-default print-access-token --cred-file=$GOOGLE_EXTERNAL_ACCOUNT_FILE

                        gcloud container clusters list --project=your-project-id
                        gcloud compute instances list --project=your-project-id
                    '''
                }
            }
        }

    }
}
