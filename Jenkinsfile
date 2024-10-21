pipeline {
    agent any
    tools {
        maven 'Maven 3.8.1' // Ensure this matches the Maven name in Global Tool Configuration
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-pass') // Single credential for username and password
        BUILD_TAG = ''
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
                            echo ""\$DOCKER_PASS" | docker login -u "\$DOCKER_USER" --password-stdin"
                        """
                    }


                    // Build the Docker image using shell command
                    sh "pwd"
                    // Replace spaces with underscores in the timestamp for the Docker tag
                    BUILD_TAG = "${BUILD_TIMESTAMP}".replace(' ', '_').replace(':', '-')
                    
                    // Build the Docker image
                    sh "docker build -t supalami/studentsurvey645:${BUILD_TAG} ."
                }
            }
        }

        stage("Pushing Image to DockerHub") {
            steps {
                script {
                    // Push the Docker image to DockerHub
                    sh "docker push supalami/studentsurvey645:${BUILD_TAG}"
                    sh 'echo "Completed pushing the image"'
                }
            }
        }

        stage("Deploying to Rancher") {
            steps {
                script {
                    // Deploy to Rancher as a single pod
                    sh "kubectl -n dev set image deployment/development container-0=supalami/studentsurvey645:${BUILD_TAG}"
                    sh 'echo "Deploying to rancher"'
                }
            }
        }
    }
}