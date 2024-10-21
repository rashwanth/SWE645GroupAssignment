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

                    // Change directory to 'myproject' before running Maven
                    dir('myproject') { 
                        // Build using Maven
                        sh "pwd"
                        sh 'mvn clean package'
                    }

                    // Extract username and password from credentials


                    // Print the timestamp for debugging
                    sh 'echo ${BUILD_TIMESTAMP}'

                    // Securely handle Docker login
                    withCredentials([usernamePassword(credentialsId: 'docker-pass', 
                                                      usernameVariable: 'DOCKER_USER', 
                                                      passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                            echo "\$DOCKER_PASS" | docker login -u "\$DOCKER_USER" --password-stdin
                        """
                    }


                    // Build the Docker image using shell command
                    sh "pwd"
                    sh "docker build -t supalami/studentsurvey645:${cleanBuildTimestamp} ."
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