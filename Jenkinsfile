pipeline {
    agent { label 'worker' }

    environment {
        IMAGE_NAME = 'prasantabdev/weather-app'  // Define your image name
    }

    stages {
        stage("Clean workspace") {
            steps {
                cleanWs()  // Clean up workspace before starting
            }
        }

        stage("Git Clone") {
            steps {
                git url: "https://github.com/prasantabdev/weather-app.git", branch: "master"
                echo "Code cloned successfully"
            }
        }

        stage("Code Build") {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."  // Build the Docker image
                echo "Code Build successfully"
            }
        }

        stage("Image Push") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhubCred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        // Log in to Docker Hub
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                        // Push the image to Docker Hub
                        sh "docker push ${IMAGE_NAME}:latest"
                        // Log out from Docker Hub after the push
                        sh 'docker logout'
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    // Deploy the Docker container
                    sh "docker run -d -p 5000:5000 ${IMAGE_NAME}:latest"
                }
            }
        }
    }

 
}
