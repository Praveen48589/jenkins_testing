@Library("Shared") _
pipeline{
    agent { label "vinod" }
    stages{
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("Code"){
            steps{
                script{
                    clone("https://github.com/Praveen48589/jenkins_testing.git", "main")
                }
            }
            
        }
        stage("Build"){
            steps{
                script{
                    build("portfolio","latest","praveen416")
                }
            }
          
            
        }
        stage("Push to the Dockerhub"){
            steps{
                echo "This is Pushing the image to Dockerhub"
                withCredentials([usernamePassword(
                    "credentialsId":"dockerHubCred",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag portfolio:latest ${env.dockerHubUser}/portfolio:latest"
                sh "docker push ${env.dockerHubUser}/portfolio:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "This is Deploying the Code"
                sh "docker compose down && docker compose up -d"
            }
        }
        
    }
}
