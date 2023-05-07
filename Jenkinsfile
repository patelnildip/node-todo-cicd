pipeline {
    agent { label "dev-server" }

    stages {
        stage('Clone code') {
            steps {
                git url: "https://github.com/patelnildip/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build and test") {
            steps {
                sh "docker build . -t node-app-test"
            }
        }
        stage("Push to docker hub") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockeHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag node-app-test  ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockeHubPass}"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                sh "docker-compose down"
                sh "docker-compose up -d --build"
            }
        }
    }
}
