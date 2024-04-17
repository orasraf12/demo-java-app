pipeline {
    agent any // Run on any available agent

    parameters {
        string(
            name: 'ENVIRONMENT',
            defaultValue: 'staging',
            description: 'Specify the environment to deploy to (dev, staging, prod)'
        )
        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,  // Set default value to true
            description: 'Should the pipeline run unit tests?'
        )
        choice(
            name: 'BRANCH',
            choices: ['master', 'develop', 'feature/my-new-feature'],
            description: 'Select the Git branch to build from'
        )
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/orasraf12/demo-java-app' 
            }
        }
        stage('Test (if not skipped)') {
            when {
                expression {
                    return !params.SKIP_BUILD_ON_PUSH // Assuming you want to skip based on SKIP_BUILD_ON_PUSH
                }
            }
            steps {
                script {
                    if (params.RUN_TESTS) {
                        sh 'mvn test' // Run tests using Maven command
                    } else {
                        sh "echo 'Tests skipped.'" // Informative message when tests are skipped
                    }
                }
            }
        }
        stage('Build Image') {
            steps {
                sh 'mvn clean install' // Build the JAR file before building the image
                sh 'mvn spring-boot:build-image'
            }
        }
        stage('Docker tag') {
            steps {
                sh 'docker tag java-petclinic-2:2.3.3 orasraf912/java-project:java-petclinic-2'
            }
        stage('Push Docker Image to Docker Hub') {
                    steps {
                        script {
                            docker.withRegistry('https://registry.hub.docker.com', 'orasraf912-dockerhub') {
                            // Push your Docker image to Docker Hub
                            docker.image('orasraf912/java-project:java-petclinic-2').push('latest')
                        }
                    }
                }
            }
    }
    post {
        always {
            archiveArtifacts '**/*.jar' // Archive the built JAR file
        }
        success {
            // Define actions on successful build (e.g., send notification)
            echo 'Build Successful!'
        }
        failure {
            // Define actions on build failure (e.g., send notification)
            echo 'Build Failed!'
        }
    }
}