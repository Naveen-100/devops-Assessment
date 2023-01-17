pipeline {
    agent any
    environment {
        dockerImage = ''
    }
    tools {nodejs "NODE JS"}
    stages {
        stage('pull the code') {
            steps {
               git credentialsId: 'jnks-pvt', url: 'git@github.com:Naveen-100/devops-Assessment.git'
            }
        }
        stage('Build and Install') {

            steps {
                script{
                sh 'npm install'
                sh 'npm test'
            }
            }
        }
        stage('sonarQube'){
            steps{
                sh 'mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=devops-123 \
                    -Dsonar.host.url=http://137.135.113.220:9000 \
                    -Dsonar.login=sqp_e046a561d4c4f57e9caa9236d24ec1469bb5ed86'
            }
        }
         stage('Docker Build') {
             steps {
                 sh 'docker build -t naveen2809/devops/nodejs:latest .'
             }
         }
         stage("Docker push"){
             steps{
                 script {
                sh "docker login -u naveen2809 -p Docker"
                sh "docker push naveen2809/devops/nodejs:latest"
                }
            }
        }
         stage('install docker'){
             steps{
                  ansiblePlaybook credentialsId: 'jnks-pvt', disableHostKeyChecking: true, inventory: 'ansible/dev.inv', playbook: 'ansible/install-docker.yml'
             }
         }
         stage('Start container'){
             steps{
                  ansiblePlaybook credentialsId: 'jnks-pvt', disableHostKeyChecking: true, inventory: 'ansible/dev.inv', playbook: 'ansible/install-image.yml'
             }
         }
    }
}
