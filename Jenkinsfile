pipeline {
    agent any

    environment {
        registryName = "saicharan492"
        registryUrl = "saicharan492.azurecr.io"
        registryCredential = "ACR" // Your Jenkins ACR credentials ID
        imageName = "devops-integration"
        imageTag = "latest"
    }

    tools {
        maven 'maven_3_5_0'
    }

    stages {
        stage('Build Maven') {
            steps {
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

        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                        export PATH=$PATH:/usr/local/bin
                        docker build -t ${registryUrl}/${imageName}:${imageTag} .
                    '''
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                        sh '''
                            export PATH=$PATH:/usr/local/bin
                            docker login -u saicharan492 -p "$dockerhubpwd"
                            docker push saicharan492/devops-integration
                        '''
                    }
                }
            }
        }

        stage('Push Image to ACR') {
            steps {
                script {
                    withDockerRegistry([url: "https://${registryUrl}", credentialsId: registryCredential]) {
                        sh '''
                            export PATH=$PATH:/usr/local/bin
                            docker push ${registryUrl}/${imageName}:${imageTag}
                        '''
                    }
                }
            }
        }
    }
}
