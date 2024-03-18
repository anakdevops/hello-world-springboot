pipeline {
    agent {label 'agent_dev' }
    tools {
        maven "maven-3.9.6"
    }

    stages {
        stage('Checkout external proj') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/anakdevops/hello-world-springboot.git'

                sh "ls -lat"
                sh "mvn clean package"
             
            }
        }
        
        stage('Build Docker') {
            steps {
                sh 'docker build -t anakdevops:$DOCKER_TAG .'
            }
        }
        stage('Build tag and push') {
            steps {
                sh 'docker tag anakdevops:$DOCKER_TAG anakdevops/java-pipeline:$DOCKER_TAG'
                sh 'docker push anakdevops/java-pipeline:$DOCKER_TAG'
            }
        }
        stage('Deploy') {
            input {
                message "Shall we deploy uat?"
                ok "Please go ahead"
            }

            agent { label 'agent_uat' }
            steps {
                sh 'docker stop anakdevops'
                sh 'docker rm anakdevops'
                sh 'docker run -d --name anakdevops -p 8344:8080 anakdevops/java-pipeline:$DOCKER_TAG'
            }
        }
    }
}
