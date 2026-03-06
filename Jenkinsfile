pipeline {
    agent { label "dev"}

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
post {
    success {
        emailext (
            subject: "Jenkins Build SUCCESS",
            body: "Your Jenkins job was successful.",
            to: "kamleshjaipur2039@gmail.com"
        )
    }
    failure {
        emailext (
            subject: "Jenkins Build FAILED",
            body: "Your Jenkins job has failed. Please check Jenkins.",
            to: "kamleshjaipur@gmail.com"
        )
    }
}
}
