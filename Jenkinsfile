pipeline { 
    agent any

    environment {
         APP_NAME = "my-calculator"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/iam-arellano/gitops-simple-calculator'                 
                }
           }
         stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
                 }
             }

         stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "raemond.arellano01@gmail.com"
                   git config --global user.email "raemond.arellano01@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                // // withCredentials([gitUsernamePassword(credentialsId: 'github_token', gitToolName: 'Default')]) {
                // //   sh "git push https://github.com/iam-arellano/gitops-simple-calculator  main"
                // }
            }
        }
        
    }
}