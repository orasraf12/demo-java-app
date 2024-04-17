
pipeline {
    agent any // Run on any available agent

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/orasraf12/demo-java-app' 
            }
        }
        stage('Build Image') {
            steps {
                sh 'ls -l'
                sh 'mvn clean install -DskipTests' // Build the project without tests (adjust as needed)
                sh 'pwd'
                sh 'mvn spring-boot:build-image'
            }
        }
            stage('Push Image') {
            steps {
                script {
                    docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD // Login to Docker registry (using credentials)
                    docker build -t $imageName . // Build the image in current directory
                    docker push $imageName // Push the image to registry
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts '**/*.jar' // Archive the built JAR file (adjust as needed)
        }
        success {
            // Define actions on successful build (e.g., send notification)
            echo 'Build Successful!' // Example message (replace with your desired action)
        }
        failure {
            // Define actions on build failure (e.g., send notification)
            echo 'Build Failed!' // Example message (replace with your desired action)
        }
    }
}