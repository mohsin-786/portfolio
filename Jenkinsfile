pipeline {
    agent any
    stages {
        stage("Clone"){
            steps{
                git url: 'https://github.com/mohsin-786/portfolio.git', branch: 'main'
            }
        }
        stage("Build"){
            steps{
                sh 'docker build . -t mohsin01/portfolio:latest'
            }
        }
        stage("Login and push"){
            steps{
                echo "Logging in and pushing to Dockerhub"
                withCredentials([usernamePassword(credentialsId:'dockerhub', usernameVariable:'USERNAME', passwordVariable:'PASSWORD')]) {
                    sh 'docker login -u $USERNAME -p $PASSWORD'  
                    sh 'docker push mohsin01/portfolio:latest'
                }
            }
        }
        stage("Deploy"){
            steps{
                sh 'docker-compose down && docker-compose up -d --no-deps --build portfolio'
            }
        }
    }
}
