pipeline {
    agent {label 'dev'}
    stages{
        stage("code"){
            steps{
                echo "code clone from github...."
                git url: 'https://github.com/sunil1397/django-notes-app.git', branch: 'main'
            }  
        }
        stage("build"){
            steps{
                echo "build code..."
                sh 'docker build -t notes-django-app .'
            }
        }
        stage("push to dockerhub"){
            steps{
                echo "push to dockerhub..."
                withCredentials([usernamePassword(credentialsId:'dockerHub', passwordVariable:'dockerHubPass', usernameVariable:'dockerHubUser')]){
                    sh "docker tag notes-django-app ${env.dockerHubUser}/notes-django-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/notes-django-app:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                echo "deploying code ....."
                sh "docker-compose down && docker-compose up -d"
            }
            
        }
    }
    
    
}
