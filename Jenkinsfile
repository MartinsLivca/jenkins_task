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
                  withAWS(region:'eu-west-1',credentials:'816952374684') {
                  sh 'echo "Uploading content with AWS creds"'
                  
                  }
                  
              }
         }
         stage('Update stack'){
             steps{
                 withAWS(region:'eu-west-1',credentials:'816952374684') {
                    cfnUpdate(stack:'jenkinstask', url:'https://s3.amazonaws.com/jenkinss3taskml/first-stack.yaml', params:['OwnerName': 'martins'])
                 }
             }
         }
     }


     post {

        success {
            echo 'Complete'
        }
        aborted {
            withCredentials([gitUsernamePassword(credentialsId: '82b12ddf-6f32-4838-ba57-c2ff87cbda1e', gitToolName: 'Default')]){
                sh '''
                git checkout main
                git pull origin main
                git branch reverted HEAD~1
                git checkout reverted first-stack.yaml
                git add .
                git commit -m "reverted yaml file"
                git branch -d reverted
                git push 
                '''
            }
 
            
        }
    }
}