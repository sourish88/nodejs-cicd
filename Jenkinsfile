pipeline {
    environment {
        registry = 'sourish88/sample-nodejs-app'
        registryCredential = 'DockerHub-Creds'
    }
    
    parameters {
        string(name: 'APP_NAME',  defaultValue: 'sample-nodejs',  description: 'Name of the container', trim: true)
        string(name: 'DOCKER_REPO',  defaultValue: 'sourish88/sample-nodejs-app',  description: 'Docker repository', trim: true)
    }

    agent none


    stages {
        // Deployment stage
        stage('Build') {
            agent {
                docker { image 'node:14' }
            }
            steps {
                sh """
                cd src
                npm install
                """
            }
        }

        stage('Build and publish image') {
            agent {
                label "master"
            }
            steps {
                script {
                    dockerImage = docker.build registry + ":1.0.$BUILD_NUMBER" 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                }
            }
        }

        stage('Deploy image') {
            agent {
                label "mac"
            }
            steps {
                sh """
                docker rm $APP_NAME
                docker run -d --name $APP_NAME -p 8081:8080 $DOCKER_REPO:1.0.$BUILD_NUMBER
                """
            }
        }
    } // End of stage
}