pipeline{
    agent any
    tools {
        jdk 'Java8' //this installs java 
        maven 'maven3.3.1' //This install maven 
       
    }
    environment {
        APP_NAME = "java-sample"
        RELEASE_NUMBER = "1.0"
        DOCKER_USER = "sweetpeaito"
        DOCKER_PASS = 'docker'//This is a secret that will be set up and used to sign into docker. it will be setup in docker hub as an access token
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE_NUMBER}"
    
    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){//check out the code from git repo into the jenkins workspace 
            steps {
                git branch: 'jenkins', credentialsId: 'github', url: 'https://github.com/techinanutshellhack/java-sample.git'
            }

        }

        stage("Build Application"){//build application 
            steps {
                sh "mvn clean package"
            }

        }

        stage("Test Application"){// test application with maven
            steps {
                sh "mvn test" 
            }

        }
        
        stage('Docker Build and Push'){
          
          steps{
            echo 'Docker build app'
            script{
                    docker.withRegistry('',DOCKER_PASS ) {
                            docker_image = docker.build "${IMAGE_NAME}"
                            docker_image.push("${IMAGE_TAG}")
                            docker_image.push("latest")
              }
            }
          }
        }

        stage ('Cleanup Artifacts') {
             steps {
                 script {
                     sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                     sh "docker rmi ${IMAGE_NAME}:latest"
                 }
             }
        }
    }
}