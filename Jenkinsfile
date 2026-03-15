pipeline{
    agent any
    stages {
        stage ("Code Cloned") {
            steps {
                git url: "https://github.com/SanketShirke/node-cicd-pipeline.git" , branch: "main"
                echo "Code Cloned"
            }
        }
        stage ("Code Build") {
            steps {
                sh "docker build . -t node-app-test"
                echo "Code build through docker"
            }
        }
        stage ("Push the Image to Docker Hub Repository") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app-test ${env.dockerHubUser}/node-app-test:latest"
                    sh "docker push ${env.dockerHubUser}/node-app-test:latest"
                }
                echo "Pushed Image to Docker Hub Repository"
            }
        }
        stage ("Deploying the application Through Docker Compose") {
            steps {
                sh "docker compose down && docker compose up -d"
                echo "Deploying the application"
            }
        }
    }
}
