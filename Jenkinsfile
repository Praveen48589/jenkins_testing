@Library('Shared') _

pipeline {
    agent { label 'vinod' }

    stages {

        stage('Hello') {
            steps {
                script {
                    hello()
                }
            }
        }

        stage('Code') {
            steps {
                script {
                    clone(
                        "https://github.com/Praveen48589/jenkins_testing.git",
                        "main"
                    )
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    dockerBuild("portfolio", "latest")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo "This is Pushing the image to DockerHub"

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerHubCred',
                        usernameVariable: 'dockerHubUser',
                        passwordVariable: 'dockerHubPass'
                    )
                ]) {
                    sh '''
                        echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                        docker tag portfolio:latest $dockerHubUser/portfolio:latest
                        docker push $dockerHubUser/portfolio:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "This is Deploying the Code"
                sh "docker compose up -d"
            }
        }
    }
}
