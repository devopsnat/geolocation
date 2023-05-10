pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '156850239407.dkr.ecr.us-east-1.amazonaws.com/devop-repository'
    registryCredential = 'jenkins-ecr'
    dockerimage = ''
    }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'https://github.com/devopsnat/geolocation.git'
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
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}
