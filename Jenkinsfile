pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/charansai15/devops-automation.git']])
                sh 'mvn clean install'
            }
        }
        stage('Check Docker') {
            steps {
                sh '''
                    export PATH=$PATH:/usr/local/bin
                    echo "PATH is: $PATH"
                    which docker
                    docker --version
                '''
            }
        }

        stage('Build docker image'){
            steps{
                script{
                   sh '''
                    export PATH=$PATH:/usr/local/bin
                    docker build -t saicharan492/devops-integration .
                '''
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                  withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u saicharan492 -p ${dockerhubpwd}'
}
                   sh 'docker push saicharan492/devops-integration'
                }
            }
        }
        
    }
}
