pipeline {
    agent { label "myagent" }
    environment {
              APP_NAME = "petclinic-working"
              GIT_USERNAME = "gani1990"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/gani1990/spc-working-CD'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:latest/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "gani1990"
                   git config --global user.email "gani87122@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-token', gitToolName: 'Default')]) {
                  sh "git push https://github.com/gani1990/spc-working-CD main"
                }
            }
        }
      
    }
}
