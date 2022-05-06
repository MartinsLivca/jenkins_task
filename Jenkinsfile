pipeline {
     agent any
     stages {
         stage('Input') {
            steps {
                input('Do you want to proceed?')
                    }
                }
               
         stage('Upload to AWS') {
              steps {
                  withAWS(region:'eu-west-1',credentials:'aws_credentials') {
                  sh 'echo "Uploading content with AWS creds"'
                  s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'first-stack.yaml', bucket:'jenkinstasks3bucket')
                  
                  }
                  
              }
         }
         stage('Update stack'){
             steps{
                 withAWS(region:'eu-west-1',credentials:'aws_credentials') {
                    cfnUpdate(stack:'jenkinstask', url:'https://s3.amazonaws.com/jenkinstasks3bucket/first-stack.yaml')
                 }
             }
         }
     }


     post {

        success {
            echo 'The stack has been updated'
        }
        aborted {
            withCredentials([gitUsernamePassword(credentialsId: 'github_credentials', gitToolName: 'Default')]){
                sh '''
                git config --global user.email "jenkins@gmail.com"
                git config --global user.name "jenkins"
                git checkout main
                git pull origin main
                git branch reverted HEAD~1
                git checkout reverted first-stack.yaml
                git add .
                git commit -m "reverted yaml file"
                git branch -d reverted
                git push 
                '''
            echo 'Commit has been reverted'
            }
 
            
        }
    }
}

