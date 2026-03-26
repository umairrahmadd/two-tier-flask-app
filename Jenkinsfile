pipeline {
    agent any;
    stages{
        stage("Cloning Code"){
            steps{
                git url: "https://github.com/umairrahmadd/two-tier-flask-app.git", branch: "master"
                echo "The code is cloning"
            }
        }
        stage("Building"){
            steps{
                sh "docker build -t two-tier-flask-app ."
                echo "The docker is building the image"
            }
        }
        stage("Test"){
            steps{
                echo "The developer test everything"
            }
        }
         stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerHubCreds',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh 'docker build -t two-tier-flask-app .'
                    sh 'docker tag two-tier-flask-app $USERNAME/two-tier-flask-app:latest'
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push $USERNAME/two-tier-flask-app:latest'
                }
            }
         }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build"
                echo "the code is successfully deployed to prod"
            }
        }
    }
}
