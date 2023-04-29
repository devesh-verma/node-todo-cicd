pipeline {
    agent {
        label "dev"
    }
    stages {
        stage('code'){
            steps {
                git url: "https://github.com/devesh-verma/node-todo-cicd.git", branch: "master"
            }
        }
        stage('Build and test'){
            steps {
                sh 'docker build . -t deveshverma/node-todo-cicd:latest'
            }
        }
        stage('Push image to hub'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'docketHub', passwordVariable: 'docketHubPassword', usernameVariable: 'docketHubUsername')]) {
                    sh "docker login -u ${env.docketHubUsername} -p ${env.docketHubPassword}"
                    sh "docker push ${env.docketHubUsername}/node-todo-cicd:latest"
                    echo "Logged in to docker hub"
                }
            }
        }
        stage('deploy'){
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
