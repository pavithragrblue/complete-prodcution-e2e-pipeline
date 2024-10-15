pipeline{
    agent any
    tools{
        jdk 'JDK 1.11'
        maven 'Maven-3.9.6'
    }
    stages{
        stage("cleanup workspace"){
            steps{
                cleanWs()
            }
        }
        stage("checkout to scm"){
            steps{
                git branch: "CICD-Demo" , credentialsId: "pavithragrblue" , url: " https://github.com/pavithragrblue/complete-prodcution-e2e-pipeline.git"
            }
        }
        stage("Build application"){
            steps{
                sh "mvn clean package"
            }
        }
    }
}
