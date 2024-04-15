pipeline {
    agent any // Run on any available agent

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/orasraf12/demo-java-app' // Replace with your git repository details
            }
        }
        stage('Build Image') {
            steps {
                sh 'mvn clean install -DskipTests' // Build the project without tests (adjust as needed)
                // Use an x86 compatible builder image (replace with actual image name)
                sh 'mvn spring-boot:build-image'
            }
        }
    }

    post {
        always {
            archiveArtifacts '**/*.jar' // Archive the built JAR file (adjust as needed)
        }
        success {
            // Define actions on successful build (e.g., send notification)
        }
        failure {
            // Define actions on build failure (e.g., send notification)
        }
    }
}