pipeline {
    agent any
    environment {
        //be sure to replace "willbla" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "chrismathew2000/spring-petclinic-jenkins"
        DOCKER_IMAGE_NAME_ECR="350423129686.dkr.ecr.us-east-1.amazonaws.com/pet-clinic-jenkins:latest"
        dockerhub=credentials('docker_hub_login')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew bootJar'
            }
        }
        /* stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build DOCKER_IMAGE_NAME_ECR
                }
            } 
        } */
        stage('Build Docker Image') {
            when {
                branch 'main'
            }
            steps {
                sh 'docker build -t  spring-petclinic-jenkins .'
                sh 'docker tag pet-clinic-jenkins:latest 350423129686.dkr.ecr.us-east-1.amazonaws.com/pet-clinic-jenkins:latest'
            }
        }
        stage('Deploy') {
            steps {
                script{
                    docker.withRegistry('350423129686.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                    app.push(DOCKER_IMAGE_NAME_ECR)
                   
                    }
                }
            }
        }
        /* stage('Deploy our image') {350423129686.dkr.ecr.us-east-1.amazonaws.com/spring-petclinic-jenkins
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', dockerhub) {
                      
                        dockerImage.push()
                    }
                }
            }
        } */

        /* stage('Build Docker Image') {
            when {
                branch 'main'
            }
            steps {
                sh 'docker build -t chrismathew2000/spring-petclinic-jenkins:v1 .'
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'main'
            }
            steps {
                sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
                sh 'docker push chrismathew2000/spring-petclinic-jenkins:v1'
            } */
        }
/*         stage('CanaryDeploy') {
            when {
                branch 'master'
            }
            environment { 
                CANARY_REPLICAS = 1
            }
            steps {
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube-canary.yml',
                    enableConfigSubstitution: true
                )
            }
        } */
/*         stage('DeployToProduction') {
            when {
                branch 'master'
            }
            environment { 
                CANARY_REPLICAS = 0
            }
            steps {
                input 'Deploy to Production?'
                milestone(1)
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube-canary.yml',
                    enableConfigSubstitution: true
                )
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube.yml',
                    enableConfigSubstitution: true
                )
            }
        } */
    }
