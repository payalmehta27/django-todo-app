pipeline {
    agent any
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/payalmehta27/django-todo-app.git" , branch: "main"
                echo "Code cloned"
            }
        }
        stage("Code Build"){
            steps{
                sh "docker build . -t django-app"
                echo "Code Build"
            }
            
        }
        stage("Pushing the images to the docker hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh"docker tag django-app:latest ${env.dockerhubuser}/django-app:latest"
                sh "docker push ${env.dockerhubuser}/django-app:latest"
                }
                echo "Pushing the images to the docker hub"
            }
        }
        stage("Deploying the application"){
            steps{
                sh "docker compose down && docker compose up -d"
                echo "Applicatiion got deployed"
            }
        }
    }
}
