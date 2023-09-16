pipeline {
    agent any
    stages {
            stage('Create Directory') {
            steps {
                script {
                    // Define the directory path
                    def directoryPath = '/home/ec2-user/test4'

                    // Create the directory
                    sh "mkdir -p ${directoryPath}"

                    // Verify if the directory was created
                    if (fileExists(directoryPath)) {
                        echo "Directory created successfully: ${directoryPath}"
                    } else {
                        error "Failed to create directory: ${directoryPath}"
                    }
                }
            }
        }
        stage('Build') {
            steps {
                echo "Building the application..."
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying the application..."
            }
        }
    }
}
