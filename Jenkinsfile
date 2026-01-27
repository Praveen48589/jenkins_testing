# for docker compose and push image to docker hub

pipeline{
    agent { label "vinod" }
    stages{
        stage("Code"){
            steps{
                echo "This is cloning the Code"
                git url: "https://github.com/Praveen48589/jenkins_testing.git", branch: "main"
                echo "successfully cloning the Code"
            }
            
        }
        stage("Build"){
            steps{
                echo "This is Building the Code"
                sh "whoami"
                sh "docker build -t portfolio:latest ."
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
                sh "docker compose up -d"
            }
        }
        
    }
}
