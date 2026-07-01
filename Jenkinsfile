pipeline {
    agent any

    stages {

        stage('Git Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Application Build') {
            steps {
                sh 'chmod +x scripts/build.sh'
                sh './scripts/build.sh'
            }
        }

        stage('Test') {
            steps {
                sh 'chmod +x scripts/test.sh'
                sh './scripts/test.sh'
            }
        }

        stage('Docker Image Build') {
            steps {
                sh "docker build -t oleksandesusakdocker/cicd-pipeline:${BUILD_NUMBER} ."
            }
        }

        stage('Docker Image Push') {
            steps {
                script {

                    sh "docker tag \
                        oleksandesusakdocker/cicd-pipeline:${BUILD_NUMBER} \
                        oleksandesusakdocker/cicd-pipeline:latest"

                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {

                        sh "docker push \
                            oleksandesusakdocker/cicd-pipeline:${BUILD_NUMBER}"

                        sh "docker push \
                            oleksandesusakdocker/cicd-pipeline:latest"
                    }
                }
            }
        }
    }
}
