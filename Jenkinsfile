pipeline {
    environment {
        registry = 'sourish88/sample-nodejs-app'
        registryCredential = 'DockerHub-Creds'
    }
    
    parameters {
        string(name: 'APP_NAME',  defaultValue: '',  description: 'Name of the container', trim: true)
        string(name: 'DOCKER_REPO',  defaultValue: '',  description: 'Docker repository', trim: true)
    }

    agent {
        label ENV_NAME
    }


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
            steps {
                script {
                    dockerImage = docker.build registry + ":1.0.$BUILD_NUMBER" 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                }
            }
        }
    } // End of stage
}