pipeline {
    agent any
    stages{
        stage("Code Clone"){
            steps{
                echo "Code Cloned"
                git url: "https://github.com/SanketShirke/django-todo-app.git" , branch: "main"
            }
        }
        stage("Code Build"){
            steps{
                echo "Code Build"
                sh "docker build . -t django-app"
            }
        }
        stage("Push the image to Docker Hub"){
            steps{
                echo "Image Push"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}" 
                sh "docker tag django-app:latest ${env.dockerhubuser}/django-app:latest"
                sh "docker push ${env.dockerhubuser}/django-app:latest"
                }
            }
        }
        stage("Deploy the Application"){
            steps{
                echo "Deployed the Application"
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
