
pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/NimishRathi/CICD-PIPELINE.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "sudo docker build -t hotel:${BUILD_NUMBER} ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"myidpassword",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "sudo docker tag hotel:${BUILD_NUMBER} ${env.dockerHubUser}/hotel:${BUILD_NUMBER}"
                sh "sudo docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "sudo docker push ${env.dockerHubUser}/hotel:${BUILD_NUMBER}"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }
}
