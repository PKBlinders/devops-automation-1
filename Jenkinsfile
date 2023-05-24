
pipeline {
    agent any
    environment {
    DOCKERHUB_CREDENTIALS = credentials('Dockerhub')
  
    }
    stages{
        stage('Compile and Clean') {
            steps {

                sh "mvn clean compile"
            }
        }
        stage('Build Maven'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                   
                    sh 'docker build -t javatechie/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                  sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                  sh "docker tag javatechie/devops-integration hamzaemi/hamza_el:99"

                   sh 'docker push hamzaemi/hamza_el:99'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'Dockerhub')
                }
            }
        }
    }
}
