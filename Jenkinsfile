pipeline{
    agent any
    tools {
        jdk 'Java 17'
        maven 'Maven 3.8.8'
    }
    environment {
        APP_NAME = "facebook-applications"
        RELEASE = "2.0.2"
        DOCKER_USER = "pavithragrblue"
        DOCKER_PASS = "docker-login"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("Jenkins-api-token")
    }
    stages{
        stage("cleanup workspace"){
        steps{
            cleanWs()
        }
    }
    stage("checkout to git"){
        steps{
            git branch: 'CICD-Demo' , credentialsId: 'github-token' , url: 'https://github.com/pavithragrblue/complete-prodcution-e2e-pipeline.git'
                    }
    }
    stage("build application"){
        steps{
            bat 'mvn clean package'
        }
    }
    stage("Test application"){
        steps{
            bat 'mvn test'
        }
    }
        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'Sonarqube-token') {
                        bat "mvn sonar:sonar"
                    }
                }
            }

        }
        
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

        }
}
}
