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
                withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'GIT_USERNAME', passwordVariable: 'github-token')]) {
                    sh '''
                        git config user.name "$GIT_USERNAME"
                        git config user.email "gani87122@gmail.com"

                        # Update the remote URL with token auth
                        git remote set-url origin https://$GIT_USERNAME:github-token@github.com/gani1990/spc-working-CD.git

                        # Add changes, commit, and push
                        git add .
                        git commit -m "Automated commit from Jenkins" || echo "Nothing to commit"
                        git push origin main
                    '''
                }
            }
            
        }
      
    }
}
