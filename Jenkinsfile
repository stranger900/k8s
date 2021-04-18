pipeline{
    agent any
    
    stages{
        stage('Git checkout'){
            steps{
                git 'https://github.com/stranger900/k8s.git'
            }
        }
        stage('update kubeconfig') {
             steps {
                 withAWS(credentials: 'aws_credentials', region: 'us-east-1') {
                     sh 'aws eks --region us-east-1 update-kubeconfig --name eks'
                 }
             }
         }
        stage('k8s apply secret'){
            steps{
                withAWS(credentials: 'aws_credentials', region: 'us-east-1') {
                    sh '/usr/local/bin/kubectl apply -f secret.yml'
                }
            }
        }
        stage('k8s apply mysql'){
            steps{
                withAWS(credentials: 'aws_credentials', region: 'us-east-1') {
                    sh '/usr/local/bin/kubectl apply -f mysql-service.yml'
                    sh '/usr/local/bin/kubectl apply -f mysql-persistent-volume-claim.yml'
                    sh '/usr/local/bin/kubectl apply -f mysql-deployment.yml'
                }
            }
        }
        stage('k8s apply wordpress'){
            steps{
                withAWS(credentials: 'aws_credentials', region: 'us-east-1') {
                    sh '/usr/local/bin/kubectl apply -f wordpress-service.yml'
                    sh '/usr/local/bin/kubectl apply -f wordpress-persistent-volume-claim.yml'
                    sh '/usr/local/bin/kubectl apply -f wordpress-deployment.yml'
                }
            }
        }
        stage('k8s outputs'){
            steps{
                withAWS(credentials: 'aws_credentials', region: 'us-east-1') {
                    sh '/usr/local/bin/kubectl get pods'
                    sh '/usr/local/bin/kubectl get svc'
                }
            }
        }
    }

}