pipeline {
    agent any
    stages{
        stage("Code Cloned"){
            steps{
                echo "Code Cloned"
                git url: "https://github.com/SanketShirke/django-todo-app.git", branch: "main"
            }
        }
        stage("Code Build"){
            steps{
                sh "docker build . -t django-app"
                echo "Code Build"
            }
        }
        stage("Push to the Docker Hub"){
            steps{
                echo "Image push to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker tag django-app:latest ${env.dockerhubuser}/django-app:latest"
                sh "docker push ${env.dockerhubuser}/django-app:latest"
                }
            }
        }
        stage("Deploy the application"){
            steps{
                sh "docker compose  up -d"
                echo "Deploy Sucesfully"
            }
        }
    }
}
