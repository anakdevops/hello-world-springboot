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
                script {
                    def branchName = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                    echo "Branch name: ${branchName}"
                }
            }
        }
        
        stage('Build Docker') {
            steps {
                sh 'docker build -t anakdevops:v1 .'
            }
        }
        stage('Build tag and push') {
            steps {
                sh 'docker tag anakdevops:v1 anakdevops/java-pipeline:v1'
                sh 'docker push anakdevops/java-pipeline:v1'
            }
        }
        stage('Deploy') {
            agent { label 'agent_uat' }
            steps {
                sh 'docker stop anakdevops'
                sh 'docker rm anakdevops'
                sh 'docker run -d --name anakdevops -p 8344:8080 anakdevops/java-pipeline:v1'
            }
        }
    }
}
