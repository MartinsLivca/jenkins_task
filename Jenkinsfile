pipeline {
     agent any
     stages {
         stage('Build') {
             steps {
                 sh 'echo "Building..."'
             }
         }      
         stage('Upload to AWS') {
              steps {
                  script {
            // Define Variable
             def USER_INPUT = input(
                    message: 'Upload to S3 Bucket?',
                    parameters: [
                            [$class: 'ChoiceParameterDefinition',
                             choices: ['no','yes'].join('\n'),
                             name: 'input',
                             description: 'Menu - select box option']
                    ])

            echo "The answer is: ${USER_INPUT}"

            if( "${USER_INPUT}" == "yes"){
                withAWS(region:'eu-west-1',credentials:'816952374684') {
                  sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'first-stack.yaml', bucket:'jenkinss3taskml')
                  }
            } else {
                //do something else
            }
        }
    }
    }
}
}