pipeline{
    agents any
    tools{
        jdk 'Java 17'
        maven 'Maven 3.8.8'
    }
    environments{
        APP_NAME = "facebook-applications"
        RELEASE = "2.0.2"
        DOCKER_USER = "pavithragrblue"
        DOCKER_PASS = "docker-login"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-{BUILD_NUMBER}"
        JENKINS_API_TOKEN = (credentialsId:"Jenkins-api-token")
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
}
}
