pipeline {
    agent any
    stages{
        stage("Code Cloned") {
            steps {
                git url: "https://github.com/SanketShirke/node-cicd-pipeline.git" , branch: "main"
                echo "Code Cloned"
            }
        }
        stage("Code Build") {
            steps {
                sh "docker build . -t node-app"
                echo "Code Build"
            }
        }
        stage("Image Pushed To Docker Repository") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app ${env.dockerHubUser}/node-app:latest"
                    sh "docker push ${env.dockerHubUser}/node-app:latest"
                }
                echo "Image Pushed To Docker Repository"
            }
        }
        stage("Code has been deployed") {
            steps {
                sh "docker compose down &&  docker compose up -d"
                echo "Code has been deployed"
            }
        }
    }
}
