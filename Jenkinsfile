pipeline {

  agent any  
   

  stages {

      stage('Checkout SCM'){
          steps{
                catchError(buildResult: 'success', stageResult: 'FAILURE') {
                git branch: "main", credentialsId: 'git_credentials', url: 'https://github.com/mahenel/sampleRepo'
                  sh '''
                   ls -la
                   '''
                }
            }
      }

      stage('Docker Image Build stage'){
            steps{
                sh 'docker build -t  samplenode:0.01 .'
            }
        }
 
       stage('Docker Image to ECR') {
            steps {
                script {
                    
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'jenkinsuser', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                
                sh("aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 647824729885.dkr.ecr.ap-south-1.amazonaws.com")
                        
                sh "docker tag samplenode:0.01 647824729885.dkr.ecr.ap-south-1.amazonaws.com/samplerepo:0.01"     
                
                sh "docker push 647824729885.dkr.ecr.ap-south-1.amazonaws.com/samplerepo:0.01"
                    }
                }
            }
        }
    

      /* stage('Image Version change'){
            steps{
            sh """#!/bin/bash
            cat  nodesample.yaml | grep image
            sed -i 's|image: .*|image: 647824729885.dkr.ecr.ap-south-1.amazonaws.com/samplerepo:0.01|' nodesample.yaml
            cat  nodesample.yaml | grep image
            """
            }
        }
        
        stage(' Deploy to kubernetes '){
            steps{
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'kubeconfig', namespace: '', serverUrl: '') {
                sh '''
                 kubectl apply -f ./nodesample.yaml
                 '''
                }
                
            }
        }*/

    }
}
