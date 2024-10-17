pipeline{
    agent any
    tools {
        jdk 'Java 17'
        maven 'Maven 3.8.8'
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
    }
}
