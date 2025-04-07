pipeline {
  agent any

  stages {
    stage('Authenticate and List GCP') {
      steps {
        withCredentials([file(credentialsId: 'WIF_CONFIG', variable: 'WIF_CREDENTIALS')]) {
          withEnv(["GOOGLE_APPLICATION_CREDENTIALS=$WIF_CREDENTIALS"]) {
            sh '''
              echo "Authenticating with Workload Identity Federation..."
              gcloud auth application-default print-access-token

              echo "Listing GCP projects..."
              gcloud projects list
            '''
          }
        }
      }
    }
  }
}
pipeline {
    agent any
    environment {
        GOOGLE_APPLICATION_CREDENTIALS = credentials('WIF_CONFIG')
    }
    stages {
        stage('Authenticate with GCP') {
            steps {
                sh '''
                    echo "Authenticating with Workload Identity Federation..."
                    gcloud auth application-default print-access-token
                '''
            }
        }

        stage('List GCP Resources') {
            steps {
                sh 'gcloud projects list'
            }
        }
    }
}
