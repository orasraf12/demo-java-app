
pipeline {
    agent any // Run on any available agent

    parameters {
          string(
              name: 'ENVIRONMENT',
              defaultValue: 'staging',
              description: 'Specify the environment to deploy to (dev, staging, prod)'
          )

          parameters {
            booleanParam(
                name: 'RUN_TESTS',
                defaultValue: true,  // Set default value to true
                description: 'Should the pipeline run unit tests?'
            )
          }

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
                if (params.RUN_TESTS) {
                    sh 'mvn clean install'
                    sh 'mvn clean install -DskipTests'
                } else {
                    sh "echo 'i in the else'"
                }
            }
        }

        stage('Build Image') {
            steps {
                sh 'ls -l'
                sh 'pwd'
                sh 'mvn clean install'
                sh 'mvn spring-boot:build-image'
            }
        }

        stage('push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.oauth.com/v1/', credentialsId: '39705250-6e95-447f-abac-fa54235a99f7') 
                    { // Replace with your registry URL if different
                        def imageName = "${params.IMAGE_NAME}" // Get image name from pipeline parameter (optional)
                        def imageTag = "${params.IMAGE_TAG}"   // Get image tag from pipeline parameter (optional)

                        if (!imageName) {
                            imageName = "java-petclinic-1" // Default image name
                        }

                        if (!imageTag) {
                            imageTag = "2.3.2" // Default image tag
                        }

                        def builtImage = docker.build(imageName + ":" + imageTag)
                        builtImage.push()
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