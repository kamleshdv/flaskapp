pipeline {
    agent any

    stages {
        stage("code") {
            steps {
                git url: "https://github.com/kamleshdv/flaskapp.git" , branch: "main"
            }
        }

        stage("build") {
            steps {
                sh "docker build -t two-tier-flask-app:latest ."
            }
        }

        stage("test") {
            steps {
                sh "docker compose up -d"
            }
        }
         stage("push to docker hub") {
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhubcred",
                passwordVariable: "dockerHubPass",
                usernameVariable: "dockerHubUser"
                )]) {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app:latest ${env.dockerHubUser}/two-tier-flask-app:latest"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
         }

        stage("deploy") {
            steps {
                echo"docker compose se deploy"
            }
        }
    }
}
