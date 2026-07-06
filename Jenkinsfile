pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Ensure mvnw has execute permissions and package the application
                sh 'chmod +x mvnw'
                sh './mvnw clean package -DskipTests'
            }
        }
        
        stage('Test') {
            steps {
                sh './mvnw test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Building Docker Image...'
                sh 'docker build -t employee-management .'

                echo 'Stopping and removing old container if it exists...'
                sh 'docker stop employee-management || true'
                sh 'docker rm employee-management || true'

                echo 'Deploying new container...'
                // Using --network="host" allows the container to connect to PostgreSQL on localhost:5433
                sh 'docker run -d --name employee-management --network="host" employee-management'
                
                echo 'Deployment successful! Application is running.'
            }
        }
    }
}
