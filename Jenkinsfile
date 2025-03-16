pipeline {
    agent any
    environment {
        // Define environment variables
        PUBLISH_FOLDER = "C:\\Jenkins\\workspace\\${JOB_NAME}\\publish" // Path to the published app
        DESTINATION_FOLDER = "D:\\WebApplication9"  // Target deployment folder
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Restoring dependencies
                    bat "dotnet restore"

                    // Building the application
                    bat "dotnet build --configuration Release"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Running tests
                    bat "dotnet test --no-restore --configuration Release"
                }
            }
        }

        stage('Publish') {
            steps {
                script {
                    // Publishing the application
                    bat "dotnet publish --no-restore --configuration Release --output ${PUBLISH_FOLDER}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying to ${DESTINATION_FOLDER}..."

                    // Copying published files to the target folder
                    bat """
                    robocopy ${PUBLISH_FOLDER} ${DESTINATION_FOLDER} /MIR /E /Z /R:3 /W:5
                    """
                    
                    // Optionally, restart IIS App Pool if needed
                    bat """
                    powershell -Command Restart-WebAppPool -Name 'WebApplication99'
                    """
                    
                    echo 'Deployment to D://WebApplication99 success!'
                }
            }
        }
    }

    post {
        success {
            echo 'Build, test, publish, and deployment successful!'
        }
        failure {
            echo 'Deployment failed. Please check the logs for details.'
        }
    }
}
