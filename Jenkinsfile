pipeline {
    agent any
    environment {
        dockerImage = ''
    }
    stages {
        stage('Poll Code Repository') {
            steps {
                git credentialsId: 'jnks-pvt', url: 'git@github.com:Naveen-100/devops-Assessment.git'
            }
        }
        stage('npm install') {
            steps {
                sh 'npm install'
            }
        }
        stage('test') {
            steps {
                sh 'npm run test'
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
        stage('docker build') {
            steps{
                script{
                    dockerImage = docker.build("naveen2809/node-1:latest")
                }
            }
        }
        stage('docker push') {
            
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-key', url: "") {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Ansible install docker'){
            steps{
                ansiblePlaybook credentialsId: 'jnks-pvt', disableHostKeyChecking: true, inventory: 'ansible/dev.inv', playbook: 'ansible/docker.yml'
            }
        }
        stage('Ansible deploy  image'){
            steps{
                ansiblePlaybook credentialsId: 'jnks-pvt', disableHostKeyChecking: true, inventory: 'ansible/dev.inv', playbook: 'ansible/deploy-image.yml', vaultCredentialsId: 'ansible-vault'
            }
        }
    }
}
