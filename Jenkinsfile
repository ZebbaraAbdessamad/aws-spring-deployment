pipeline {
    agent any
      tools {
        // Specify the name of the Maven installation defined in Jenkins
        maven 'Maven 3.6.3'
    }

    environment {
        EC2_HOST = '3.81.236.160' // Update with your EC2 instance's IP address
        EC2_USER = 'ec2-user'     // Update with your EC2 instance's SSH username
        SSH_CREDENTIALS_ID = '3.81.236.160' // Update with your SSH key credential ID
    }

    stages {

        stage('Build') {
            steps {
                // Build your Spring Boot application
                sh 'mvn clean package' // Adjust your build command
            }
        }

        stage('Deploy') {
            steps {
                // Copy the JAR file to the EC2 instance
                sh "scp -i ${SSH_CREDENTIALS_ID} target/*.jar ${EC2_USER}@${EC2_HOST}:~/"

                // SSH into the EC2 instance and deploy the application
                sh "ssh -i ${SSH_CREDENTIALS_ID} ${EC2_USER}@${EC2_HOST} 'nohup java -jar ~/*.jar > app.log 2>&1 &'"
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
