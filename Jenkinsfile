pipeline {
    agent any
    environment {
        DOCKERHUB_PASS = credentials('docker-pass')
    }
    stages {
        stage("Building the Student Survey Image") {
            steps {
                script {
                    checkout scm
                    sh 'mvn clean package'
                    sh 'echo ${BUILD_TIMESTAMP}'
                    // Extract username and password from credentials
                    def dockerHubUsername = "${DOCKERHUB_CREDENTIALS_USR}"
                    def dockerHubPassword = "${DOCKERHUB_CREDENTIALS_PSW}"

                    // Login to DockerHub using credentials
                    sh "docker login -u ${dockerHubUsername} -p ${dockerHubPassword}"

                    // Build the Docker image
                    def customImage = docker.build("hekme5/studentsurvey645:${BUILD_TIMESTAMP}")
                }
            }
        }
        stage("Pushing Image to DockerHub") {
            steps {
                script {
                    sh 'echo Completed 1'
                    // sh 'docker push hekme5/studentsurvey645:${BUILD_TIMESTAMP}'
                }
            }
        }
        stage("Deploying to Rancher as single pod") {
            steps {
                // sh 'kubectl set image deployment/stusurvey-pipeline stusurvey-pipeline=hekme5/studentsurvey645:${BUILD_TIMESTAMP} -n jenkins-pipeline'
                sh 'echo Completed 2'
            }
        }
        stage("Deploying to Rancher with load balancer") {
            steps {
                // sh 'kubectl set image deployment/studentsurvey645-lb studentsurvey645-lb=hekme5/studentsurvey645:${BUILD_TIMESTAMP} -n jenkins-pipeline'
                sh 'echo Completed 3'
            }
        }
    }
}
