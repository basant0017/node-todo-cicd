pipeline {
    agent any
    
    stages{
        stage('Code'){
            steps{
                git url: 'https://github.com/basant0017/node-todo-cicd.git', branch: 'main' 
            }
        }
        stage('Build and Test'){
            steps{
                sh 'docker build . -t trainwithshubham/node-todo-test:latest'
            }
        }
        stage('Push'){
            steps{
                withCredentials([string(credentialsId: 'docker', variable: 'docker')]) {
        	     sh 'docker login -u 8875022556 -p ${docker}'
                 sh 'docker push trainwithshubham/node-todo-test:latest'
                }
            }
        }
        stage('Deploy'){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
