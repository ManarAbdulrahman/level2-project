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
              
              	build job: 'web-service-pipeline/master', wait: false
		build job: 'role-service-pipeline/master', wait: false
		build job: 'person-service-pipeline/master', wait: false
		build job: 'office-service-pipeline/master', wait: false
		build job: 'department-service-pipeline/master', wait: false

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
