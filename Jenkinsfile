pipeline {
    agent {
        docker { 
            image 'node:22.14.0-alpine3.21'
            args '--user root'
            }
    }
    parameters {
        choice(
            name: 'ENVIRONMENT', 
            choices: ['dev', 'staging', 'prod'], 
            description: 'Select deployment environment'
        )
    }   
    stages {

        stage('Checkout') {
            steps {
                 git branch: 'main', url: 'https://github.com/OmerKH/Jenkins-Exercise.git'
            }             
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run') {
            steps {
            sh 'npm start &'

            }
        }
        stage('Deploy') {
            stage('deploy') {
                environment {
                    AWS_ACCESS_KEY_ID = credentials('jenkins_aws_access_key_id')
                    AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws_secret_access_key')
                }
                steps {
                    script {
                       echo 'deploying docker image...'
                       sh 'kubectl create deployment nginx-deployment --image=nginx'
                }
            }
        }
    }
}
