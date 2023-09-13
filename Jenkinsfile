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
        DOCKER_PASS = 'dockertoken'
        IMAGE_NAME = "${pavithragrblue}" + "/" + "${pipeline}"
        IMAGE_TAG = "${RELEASE}-${b82b9f3}"
        JENKINS_API_TOKEN = credentials("11e864191edc762a8d93fb7d7da18f37c9")

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
    stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonarqubetoken') {
                        sh "mvn sonar:sonar"
                    }
                }
            }

        }
      stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',dockertoken) {
                        docker_image = docker.build "${Eiffelpic}"
                    }

                    docker.withRegistry('',dockertoken) {
                        docker_image.push("${RELEASE}")
                        docker_image.push('latest')
                    }
                }
            }

        }
       }
}
