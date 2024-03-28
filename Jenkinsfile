pipeline {
    agent {
        label "jenkins-agent"
    }

    tools{
        maven "Maven3"
    }
    environment{
        APP_NAME = "devops-manifest-pipeline"
    }

    stages {
        stage('Clean Up Worskapce') {
            steps {
                //Builtin method for leaning up workspace
                deleteDir()
            }
        }

        stage('Checkout from SCM-github') {
            steps {
                //SCM used to fetch our jenkins file 
               git branch: 'main', credentialsId: 'github', url: 'https://github.com/ashlesh19/devops_manifest'
            }
        }


        stage('update the deployment from Tags') {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }


        stage('Push the changed feployment file to Git') {
            steps {
                sh """
                    git config --global user.name "ashlesh19"
                    got config --global user.email "mithur.ashlesh@gamil.com"
                    git add deployment.yaml
                    git commit -m "updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-pat', gitToolName: 'Defaut')]) {
                    sh "git push https://github.com/ashlesh19/devops_manifest main"
                }

            }
        }
    
    }

}


