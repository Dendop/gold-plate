


pipeline{
    stages{

        stage("Clean up containers"){
            steps{
                sh "docker container prune -a"
            }
        }
        stage("Create a network"){
            steps{
                sh "docker network create app-network"
            }
        }
        stage("Create flask-app image"){
            steps{
                sh "cd flask-app"
                sh "docker build -t flaskimg ."
            }
        }
        stage("Create nginx image"){
            steps{
                sh "cd nginx"
                sh "docker build -t nginx ."
            }
        }
        stage("Run the flask-app container"){
            steps{
                sh "docker run -d \
                --name flask-app \
                --network app-network \
                -p 5500:5500 \
                flask-app"
            }
        }
        stage("Run the nginx container"){
            steps{
                sh "docker run -d \
                --name nginx \
                --network app-network \
                -p 80:80 \
                nginx"
            }
        }
        stage("Checking"){
            steps{
                sh "-curl localhost"
            }
        }
    }
}