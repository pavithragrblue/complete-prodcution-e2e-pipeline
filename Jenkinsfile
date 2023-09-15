pipeline{
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
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
    stage("Deploy") {
            steps {
                script {
                    withAWS(credentials: 'AWS Credentials') {
                       sh 'aws ec2 describe-instances --region us-east-1'
                    
                    withKubeConfig(clusterName: 'DEV_cluster', contextName: 'arn:aws:eks:us-east-1:746816133431:cluster/DEV_cluster', credentialsId: 'kubeconfig', namespace: 'default', serverUrl: 'https://2DB487353827CA393967CD9B4EE9B3FF.sk1.us-east-1.eks.amazonaws.com') {
                         sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'  
                         sh 'chmod u+x ./kubectl' 
                        sh 'aws eks update-kubeconfig --name DEV_cluster --region us-east-1'
                         sh './kubectl get nodes'
                    }
                 }
                }
            }
       }
}
}
