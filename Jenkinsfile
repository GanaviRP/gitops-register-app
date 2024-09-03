pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "register-apps-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'Github', url: 'https://github.com/GanaviRP/gitops-register-app.git'
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
                   git config --global user.name "GanaviRP"
                   git config --global user.email "ganavirp48@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'Github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/GanaviRP/gitops-register-app.git"
                }
            }
        }
      
    }
}
