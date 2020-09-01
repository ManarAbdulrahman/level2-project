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
              container('installer') {
              
              parallel(
               a:{build job: 'web-service-pipeline'} 
               b:{build job: 'role-service-pipeline'} 
               c:{build job: 'person-service-pipeline'}
               d:{build job: 'office-service-pipeline'}
               e:{build job: 'department-service-pipeline'}             
              )
             

              }
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

