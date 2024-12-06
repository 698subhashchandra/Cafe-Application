pipeline {
    agent any
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage("Clone Code") {
            steps {
                git url: "https://github.com/698subhashchandra/Cafe-Application.git", branch: "main"
            }
        }
        stage("Build and Test") {
            steps {
                sh "docker build . -t cafe-app"
            }
        }
        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "docker tag cafe-app ${env.dockerHubUser}/cafe-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/cafe-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                sh " docker-compose down && docker-compose up -d"
            }
        }
    }
}
