pipeline {
    agent { label "myagent" }
    environment {
              APP_NAME = "petclinic-working"
              //GIT_USERNAME = "gani1990"
              //GIT_TOKEN = credentials("github-token")
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
                sshagent(credentials: ['github-ssh']) {
                    sh '''
                        git config user.name "gani1990"
                        git config user.email "gani87122@gmail.com"

                        # Set SSH remote
                        git remote set-url origin git@github.com:gani1990/spc-working-CD.git

                        git add .
                        git commit -m "Automated commit from Jenkins" || echo "Nothing to commit"
                        git push origin main
                    '''
                }
            }
        }
      
    }
}
