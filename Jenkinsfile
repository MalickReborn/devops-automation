pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/MalickReborn/devops-automation/']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t malickguess/devops-automation .'
                }
            }
        }
        stage('Push image to Hub'){
            environment {
            DOCKERHUB_CREDENTIALS = credentials('dockerhub')
            }
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
              }
            }
            stage('Push') {
              steps {
                sh 'docker push malickguess/devops-automation'
              }
            }
        }

    }
}
