pipeline{
    agent any
    tools {
        jdk 'Java 17'
        maven 'Maven 3.8.8'
    }
    environment {
        APP_NAME = "complete-e2e-app-success"
        RELEASE = "1.0.1"
        DOCKER_USER = "pavithragrblue"
        DOCKER_PASS = 'docker-login'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("Jenkins-api-token")

    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'pavithragrblue', url: 'https://github.com/dmancloud/complete-prodcution-e2e-pipeline'
            }

        }

        stage("Build Application"){
            steps {
                bat "mvn clean package"
            }

        }

        stage("Test Application"){
            steps {
                bat "mvn test"
            }

        }
        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'Sonarqube-token') {
                        bat 'mvn clean verify sonar:sonar'
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
        stage("Trivy Scan") {
            steps {
                script {
		   bat ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image /pavithragrblue/complete-e2e-app-success:1.0.1-28 --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
                }
            }

        }


        
    }
}
