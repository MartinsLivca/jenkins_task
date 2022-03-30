pipeline {
    agent any
    }
    stages {
       stage('Build') {
            steps {
                echo 'Building..'
            }
        }
         stage('Deploy') {
            steps {
                  withAWS(region:'eu-west-1',credentials:'816952374684') {
                  sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'first-stack.yaml', bucket:'jenkinss3taskml')
            }
        }
    }
}
}
