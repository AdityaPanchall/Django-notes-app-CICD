//Syntax for shared Library 
@Library("Shared") _

pipeline{
    agent { label "vinod" }
    
    //Accessing the function of shared library 
    stages{
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        
        stage("Clone the Code"){
            steps{
                echo "Cloning the code"
                git url: "Your_Github_URL", branch: "main"
                echo "code cloned successfully"
            }
        }
        
        stage("Build and Test.."){
            steps{
                echo "Building the docker image"
                sh "docker build -t notes-app-cicd:latest ."
            }
        }
            
        stage("Push Build to Docker Hub"){
            steps{
                echo "Pushing Build to DockerHub.."
                withCredentials([
                    usernamePassword(
                        credentialsId: "DockerhubCred",
                        passwordVariable: "dockerHubPass",
                        usernameVariable: "dockerHubUser"
                    )
                ]){
                    sh "docker tag notes-app-cicd ${env.dockerHubUser}/notes-app-cicd:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/notes-app-cicd:latest"
                }
            }
        }
        
        stage("Deploy"){
            steps{
                   echo "This is deploying the code"
                   sh "docker-compose down && docker-compose up -d"
             }
        }
    }
}
