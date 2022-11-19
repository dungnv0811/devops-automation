pipeline {
    agent any
    tools {
        maven 'maven_3_8_6'
    }
    stages {
        stage('Build Maven') {
            echo "Pulling changes from the branch ${params.branch}"
            steps {
                checkout([$class: 'GitSCM', branches: [[name: "*/${params.branch}"]], extensions: [], userRemoteConfigs: [[url: 'https://github.com/dungnv0811/devops-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t dungnv08111/devops-integration .'
                }
            }

        }
        stage('Push image to hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhubp-pwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u dungnv08111 -p ${dockerhubpwd}'
                    }
                    sh 'docker push dungnv08111/devops-integration'
                }
            }
        }
    }
}