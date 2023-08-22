pipeline{
    agent any
    stages{
        stage("Code Cloned"){
            steps{
                git url: "https://github.com/skrahil/two-tier-flask-app.git", branch: "master"
                echo "Code cloned"
            }
        }
        stage("Code build"){
            steps{
                sh "docker build . -t two-tier-app"
                echo "Code build"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"Dhubpass",usernameVariable:"Dhubuser")]){
                    sh "docker login -u ${env.Dhubuser} -p ${env.Dhubpass}"
                    sh "docker tag two-tier-app ${env.Dhubuser}/two-tier-app:latest"
                    sh "docker push ${env.Dhubuser}/two-tier-app:latest"
                }    
           }
        }
        stage("Code build and deployed"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo "Code Deployed"
            }
        }
   }
}
