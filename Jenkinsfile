pipeline {
    agent {label 'agent_dev' }
    tools {
        maven "maven-3.9.6"
    }

    stages {
        stage('Clone & compile') {
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
        stage('Cleanup') {
      steps {
        cleanWs()  // Cleans workspace after all previous stages
      }
    }
        stage('Deploy') {
            input {
                message "Deploy ke uat?"
                ok "Ya"
            }

            agent { label 'agent_uat' }
            steps {
    script {
        def containerExists = sh(script: 'docker ps -a | grep anakdevops', returnStatus: true)
        if (containerExists == 0) {
            echo "Container 'anakdevops' tidak ditemukan. Melakukan 'docker run'..."
            sh 'docker run -d --name anakdevops -p 8344:8080 anakdevops/java-pipeline:$DOCKER_TAG'
        } else {
            echo "Container 'anakdevops' ditemukan. Menghapus dan menjalankan kembali..."
            sh 'docker stop anakdevops || true' // Stop container if exists, ignoring errors if not found
            sh 'docker rm anakdevops || true'   // Remove container if exists, ignoring errors if not found
            sh 'docker run -d --name anakdevops -p 8344:8080 anakdevops/java-pipeline:$DOCKER_TAG'
        }
    }
}
        }
    }

   
    
}
