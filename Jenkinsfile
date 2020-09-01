pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: installer
    image: bryandollery/terraform-packer-aws-alpine 
    command: 
    - bash
    tty: true
"""
    }
  }
  stages {
      stage("create namespace") {
          steps {
              container('installer') {
                  sh 'kubectl create namespace project2'
              }
          }
      }
      stage("apply") {
          steps {
              parallel(
               a:{
                 container('installer') {
               build job: 'web-service-pipeline'
                 }
               }
               ,
               b:{
                 container('installer') {
               build job: 'role-service-pipeline'
                 }
               }
                ,
               c:{
                 container('installer') {
               build job: 'person-service-pipeline'
                 }
               }
               ,
               d:{  
               container('installer') {
               build job: 'office-service-pipeline'
                 }
               }
               ,
               e:{   
               container('installer') {
               build job: 'department-service-pipeline'
                 }
               }             
              )
             

            
          }
      }
      stage("ingress") {
          steps {
              container('installer') {
              
                  sh 'kubectl -f ingress.yaml apply -n project2'
              }
          }
      }
      
  }

}

