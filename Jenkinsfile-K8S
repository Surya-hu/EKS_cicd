
pipeline {
   tools {
        maven 'maven3'
    }
    agent any
    environment {
        registry = "767397913675.dkr.ecr.ap-southeast-1.amazonaws.com/myecrrepo"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/Surya-hu/EKS_cicd.git']]])     
            }
        }
      stage ('Build') {
          steps {
            sh 'mvn clean install'           
            }
      }
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry 
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 767397913675.dkr.ecr.ap-southeast-1.amazonaws.com'
                sh 'docker push 767397913675.dkr.ecr.ap-southeast-1.amazonaws.com/myecrrepo:latest'
         }
        }
      }

       stage('K8S Deploy') {
            sh 'kubectl apply -f  eks-deploy-k8s.yaml'
               }
       }
    }


