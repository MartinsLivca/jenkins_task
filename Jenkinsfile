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
                withCredentials([gitUsernamePassword(credentialsId: '82b12ddf-6f32-4838-ba57-c2ff87cbda1e', gitToolName: 'Default')]) {
                sh '''
                git checkout main
                git pull
                git add .
                git branch reverted HEAD~10
                 git add .
                git commit -a -m "yes"
                 git add .
                git merge reverted -m "yes"
                 git add .
                git commit -a -m "yes"
                 git add .
                git branch -d reverted
                 git add .
                git commit -a -m "yes"
                 git add .
                git push
                '''
            }
            }
        }
    }
    }
}
}