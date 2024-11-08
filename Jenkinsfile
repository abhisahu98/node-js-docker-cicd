pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/abhisahu98/node-js-docker-cicd.git'
        IMAGE_NAME = 'nodejs-app'
        IMAGE_TAG = 'latest'
        CONTAINER_NAME = 'nodejs-app-container'
        APP_PORT = '3000'
        INVENTORY_FILE = 'hosts'  // Inventory file for Ansible
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                git url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container from the built image
                    sh 'docker run -d -p ${APP_PORT}:${APP_PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}:${IMAGE_TAG}'
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                script {
                    // Trigger the Ansible playbook for deployment
                    sh "ansible-playbook -i ${INVENTORY_FILE} ansible.yml"
                }
            }
        }
    }

    post {
        always {
            // Clean up the Docker container after the pipeline runs
            sh 'docker rm -f ${CONTAINER_NAME}'
        }
    }
}
