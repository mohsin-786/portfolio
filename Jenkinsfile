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
                sh 'docker build . -t mohsin01/portfolio:${BUILD_NUMBER}'
            }
        }
        stage("Login and push"){
            steps{
                echo "Logging in and pushing to Dockerhub"
                //withCredentials([usernamePassword(credentialsId:'dockerhub', usernameVariable:'USERNAME', passwordVariable:'PASSWORD')]) {
                  //  sh 'docker login -u $USERNAME -p $PASSWORD'  
                    sh 'docker push mohsin01/portfolio:${BUILD_NUMBER}'
               // }
               withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'docker login -u $USER -p $PASS'  
                    sh 'docker push mohsin01/portfolio:${BUILD_NUMBER}'
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
