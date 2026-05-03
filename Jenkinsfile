pipeline {
    agent { label "jenkinsAgent" }

    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "latest"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/elkadn/register-app.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "Adnane"
                   git config --global user.email "tonemail@example.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest" || echo "No changes"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/elkadn/register-app.git main"
                }
            }
        }
    }
}
