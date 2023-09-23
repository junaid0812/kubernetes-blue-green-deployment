pipeline {
    agent any
    stages {
        
         stage('Logging into AWS') {
             steps {
               withCredentials([[
               $class: 'AmazonWebServicesCredentialsBinding',
               credentialsId: 'GO-TASK',
               accessKeyVariable: 'AWS_ACCESS_KEY_ID',
               secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
            ]]) 
            {
                echo "gotask"
                sh "aws configure list"
                sh 'aws configure set region ap-south-1'
                
            }
        }
         }
         
         stage('Code Checkout') {
            steps {
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT-TOKEN', url: 'https://github.com/junaid0812/kubernetes-blue-green-deployment.git']])
                        }
        }
        
      
      stage('Deploy on EKS Cluster') { 
             steps { 
              withCredentials([[ 
              $class: 'AmazonWebServicesCredentialsBinding', 
              credentialsId: 'GO-TASK', 
              accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
              secretKeyVariable: 'AWS_SECRET_ACCESS_KEY' 
            ]]) { 
                // sh "aws configure list"
                sh "curl -o kubectl https://s3.us-west-2.amazonaws.com/amazon-eks/1.23.7/2022-06-29/bin/linux/amd64/kubectl"
                sh "chmod +x ./kubectl"
                sh "sudo mv ./kubectl /usr/local/bin/kubectl"
                sh "aws eks --region ap-south-1 update-kubeconfig --name gohash"
                sh "kubectl get po"
                sh ('kubectl apply -f kubernetes-YAML/blue-deployment.yaml')
                sh ('kubectl apply -f kubernetes-YAML/blue-service.yaml')
                  
          } }
        } 

    }
}
