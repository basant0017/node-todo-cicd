pipeline {
    agent any
    
    environment {
        dockerRun = "docker-compose down && docker-compose up -d" 
    }
    
    stages {
        stage('Code') {
            steps {
                git url: 'https://github.com/basant0017/node-todo-cicd.git', branch: 'main' 
            }
        }
        
        stage('Build and Test') {
            steps {
            //    sh 'docker build . -t 8875022556/node-todo-test:latest'
                  sh 'docker build -t $JOB_NAME:v1.$BUILD_ID .'
                  sh 'docker tag $JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:v1.$BUILD_ID'
                  sh 'docker tag $JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:latest'
            }
        }
        
        stage('Push') {
            steps {
                withCredentials([string(credentialsId: 'docker', variable: 'docker')]) {
                    sh 'docker login -u 8875022556 -p ${docker}'
                //    sh 'docker push 8875022556/node-todo-test:latest'
                   
                    sh 'docker  push 8875022556/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker  push 8875022556/$JOB_NAME:latest'
                    sh 'docker  rmi $JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:latest'
                }
            }
        }
        
        stage("Deployment") {
            steps {
                sshagent(['ssh-agent']) {
                   sh "ssh -tt -o StrictHostKeyChecking=no almalinux@15.235.147.96 ${env.dockerRun}"          
                }
            //    sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
