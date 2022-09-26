pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '076892551558.dkr.ecr.us-west-2.amazonaws.com/jenkins_repository'
    registryCredential = 'jenkins-user'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'master', url: 'https://github.com/durrello/geolocation.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-west-2:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}
