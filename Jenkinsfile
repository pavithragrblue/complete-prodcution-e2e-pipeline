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
    }
}
