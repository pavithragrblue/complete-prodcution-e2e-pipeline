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
        stage("Trivy Scan") {
            steps {
                script {
		   bat 'docker run --rm -v C:/trivy-cache:/root/.cache/ -v //./pipe/docker_engine://./pipe/docker_engine aquasec/trivy image pavithragrblue/facebook-applications:2.0.2-14 --no-progress --scanners vuln --exit-code 0 --severity HIGH,CRITICAL --format table'
                }
            }

        }
	    stage ('Cleanup Artifacts') {
            steps {
                script {
                    bat "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    bat "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }


        stage("Trigger CD Pipeline") {
            steps {
                script {
                    bat "curl -v -k --user admin:${Jenkins-api-token} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'https://jenkins.dev.dman.cloud/job/gitops-complete-pipeline/buildWithParameters?token=gitops-token'"
                }
            }

        }


}
}
