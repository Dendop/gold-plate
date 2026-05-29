pipeline{
    agent any
    stages{

        stage("Clean up containers"){
            steps{
                sh "docker rm -f flask-app || true"
                sh "docker rm -f nginx || true"
                sh "docker container prune -f"
            }
        }
        stage("Create a network"){
            steps{
                
                sh "docker network create app-network || true"
            }
        }
        stage("Create flask-app image"){
            steps{
                sh "cd flask-app && docker build -t flaskimg ."

            }
        }
        stage("Create nginx image"){
            steps{
                sh "cd nginx && docker build -t nginximg ."

            }
        }
        stage("Run the flask-app container"){
            steps{
                sh """
                docker run -d \
                --name flask-app \
                --network app-network \
                -p 5500:5500 \
                flaskimg
                """
            }
        }
        stage("Run the nginx container"){
            steps{
                sh """
                docker run -d \
                --name nginx \
                --network app-network \
                -p 80:80 \
                nginximg
                """
            }
        }
        stage("Checking"){
            steps{
                sh "sleep 5 && curl -i http://localhost"
            }
        }
    }
}