pipeline{
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "pavithragrblue"
        DOCKER_PASS = 'Dockertoken'
        IMAGE_NAME = "${pavithragrblue}" + "/" + "${pipeline}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("jenkinsapitoken")

    }
stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/dmancloud/complete-prodcution-e2e-pipeline'
            }

        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

        }

        stage("Test Application"){
            steps {
                sh "mvn test"
            }

        }
   
      stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',Dockertoken) {
                        docker_image = docker.build "${Eiffelpic}"
                    }

                    docker.withRegistry('',Dockertoken) {
                        docker_image.push("${RELEASE}")
                        docker_image.push('latest')
                    }
                }
            }

        }
       }
}
