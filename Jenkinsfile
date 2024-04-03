pipeline {
 
    tools {
        jdk "jdk17"
        maven "M3" 
    }
 
    environment {
        registry = '257307634175.dkr.ecr.ap-northeast-2.amazonaws.com/project04-ecr'
        registryCredential = 'AWS Credential ID'
        app = ''
    }
 
    agent any
 
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    app = docker.build("search/sentry-kafka-consumer:${version}", "--build-arg ENVIRONMENT=${env} .")
                }
            }
        }
        stage('Push Image') {
            steps {
                script{
 
                    docker.withRegistry("https://" + registry, "ecr:ap-northeast-2:" + registryCredential) { 
                        app.push("${version}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
