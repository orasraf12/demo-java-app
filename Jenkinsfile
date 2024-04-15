
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
                sh 'mvn clean test -e'
                // sh 'mvn clean install -DskipTests' // Build the project without tests (adjust as needed)
                // // Use an x86 compatible builder image (replace with actual image name)
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
            echo 'Build Successful!' // Example message (replace with your desired action)
        }
        failure {
            // Define actions on build failure (e.g., send notification)
            echo 'Build Failed!' // Example message (replace with your desired action)
        }
    }
}