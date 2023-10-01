pipeline {
    agent any
    
    environment {
        IMAGE_TAG="${BUILD_NUMBER}"
    }
    
    stages {
        stage("Clone"){
            steps{
                git url: 'https://github.com/mohsin-786/portfolio.git', branch: 'main'
            }
        }
        stage("Build"){
            steps{
                sh 'docker build . -t mohsin01/portfolio:${IMAGE_TAG}'
            }
        }
        stage("Login and push"){
            steps{
                echo "Logging in and pushing to Dockerhubby"
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'docker login -u $USER -p $PASS'  
                    sh 'docker push mohsin01/portfolio:${IMAGE_TAG}'
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
