pipeline {
    agent any
    tools {
        maven 'Maven 3.8.1' // Ensure this matches the Maven name in Global Tool Configuration
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-pass') // Single credential for username and password
    }
    stages {
        stage("Building the Student Survey Image") {
            steps {
                script {
                    checkout scm

                    dir('myproject') { 
                        // Build using Maven
                        sh 'mvn clean package'
                    }
                    // Extract username and password from credentials
                    def dockerHubUsername = "${DOCKERHUB_CREDENTIALS_USR}"
                    def dockerHubPassword = "${DOCKERHUB_CREDENTIALS_PSW}"

                    // Print the timestamp for debugging
                    sh 'echo ${BUILD_TIMESTAMP}'

                    // Login to DockerHub using credentials
                    sh "docker login -u ${dockerHubUsername} -p ${dockerHubPassword}"

                    // Build the Docker image
                    def customImage = docker.build("${dockerHubUsername}/studentsurvey645:${BUILD_TIMESTAMP}")
                }
            }
        }

        stage("Pushing Image to DockerHub") {
            steps {
                script {
                    // Push the Docker image to DockerHub
                    // sh "docker push hekme5/studentsurvey645:${BUILD_TIMESTAMP}"
                    sh 'echo Completed 1'
                }
            }
        }

        stage("Deploying to Rancher as single pod") {
            steps {
                script {
                    // Deploy to Rancher as a single pod
                    // sh "kubectl set image deployment/stusurvey-pipeline stusurvey-pipeline=hekme5/studentsurvey645:${BUILD_TIMESTAMP} -n jenkins-pipeline"
                    sh 'echo Completed 2'
                }
            }
        }
        
        stage("Deploying to Rancher with load balancer") {
            steps {
                script {
                    // Deploy to Rancher with load balancer
                    // sh "kubectl set image deployment/studentsurvey645-lb studentsurvey645-lb=hekme5/studentsurvey645:${BUILD_TIMESTAMP} -n jenkins-pipeline"
                    sh 'echo Completed 3'
                }
            }
        }
    }
}