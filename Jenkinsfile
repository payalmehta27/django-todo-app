pipeline {
    agent any
    stages{
        stage ("Code Cloned"){
            steps{
                git url: "https://github.com/SanketShirke/django-todo-app.git", branch: "main"
                echo "Code Cloned"
            }
        }
        stage ("Code Build"){
            steps{
                sh "docker build . -t node-app-demo"
                echo "Code Build"
            }
        }
        stage ("Push to Repository"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerhubUser")]){
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app-demo ${env.dockerhubUser}/node-app-demo:latest"
                    sh "docker push ${env.dockerhubUser}/node-app-demo:latest"
                }
                echo "Push on docker hub"
            }
        }
        stage ("Deploy the project"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo "Deployed Sucessfully"
            }
        }
    }   
}
