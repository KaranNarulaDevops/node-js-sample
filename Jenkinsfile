pipeline {
    agent any
    
    triggers {
        pollSCM('H/2 * * * *')
    }
    
    environment {
    ECR_REPO = 'public.ecr.aws/h2w6o4h2/devops24'
    Branch_name = "main"
    
    }

    stages {
        stage('Stage-1: Fetching code') {
            steps {
                // Pulling code from Github Branch to Local Workspace
                git branch: "master", url: "https://github.com/KaranNarulaDevops/node-js-sample.git"
                // git branch: "$Branch_name", url: "https://github.com/KaranNarulaDevops/node-js-sample"
            }
        }
        stage('Stage-2: Build Code') {
            steps {
                nodejs("$nodejs"){ //Node Version
                    echo 'Buidling Code....'
                    sh 'node -v'
                    sh 'npm install'
                    }
                }
            }
        stage('Stage-3: Building Docker Image') {
            steps {
                echo 'Building Artifact...'
                sh "sudo docker build -t node-app:v${env.BUILD_ID} ." // Creating docker image
                 echo 'Artifact Builded successfully'
                }
            }
        stage('Stage-4: Logging the image on AWS ECR') {
            steps {
                sh "aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REPO}"
                echo "Logged in ECR Successfully"
                }
            }
        stage('Stage-5: Tagging in the image on AWS ECR') {
            steps {
                sh "sudo docker tag node-app:v${env.BUILD_ID} ${ECR_REPO}:v${env.BUILD_ID}"
                echo "Artifact Tagged Successfully"
                }
            }
        stage('Stage-6: Pushing the image on AWS ECR') {
            steps {
                sh "docker push ${ECR_REPO}:v${env.BUILD_ID}"
                echo "Artifact Pushed Successfully"
                }
            }
    }
        post { 
         always { cleanWs() } }
}
