
pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
        FRONTEND_IMAGE = 'spygram/todo-fe'
        BACKEND_IMAGE = 'spygram/todo-be'
        FRONTEND_TAG = 'latest'
        BACKEND_TAG = 'latest'
    }
    
    stages {
        stage("SCM") {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Spygram/mern-todo-app.git']])
            }
        }
        
        stage("build docker image"){
            steps{
                sh "docker compose up -d --build"
            }
        }
	
        stage("Push image"){
            steps{
                script{
                    docker.withRegistry('', 'dockerhub-credentials-id') {
                        sh 'docker push $FRONTEND_IMAGE:$FRONTEND_TAG'
                    }
                
                    docker.withRegistry('', 'dockerhub-credentials-id') {
                        sh 'docker push $BACKEND_IMAGE:$BACKEND_TAG'
                    }
            
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}	

