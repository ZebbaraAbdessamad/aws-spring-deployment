pipeline {
    agent any
    tools {
        // Specify the name of the Maven installation defined in Jenkins
        maven 'Maven 3.6.3'
    }

    environment {
        EC2_HOST = '34.235.144.63' // Update with your EC2 instance's IP address
        EC2_USER = 'ec2-user'     // Update with your EC2 instance's SSH username
        JAR_FILE_NAME = 'myapp-0.0.1-SNAPSHOT.jar' 
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
        script {
            // Add the host key to known_hosts file in Jenkins
            sh "ssh-keyscan -H $EC2_HOST >> ~/.ssh/known_hosts"

            // Copy the JAR file to the EC2 instance using scp with -i
            sh "scp -i \$WORKSPACE/zebbara-ssh-aws.pem -v target/\$JAR_FILE_NAME \$EC2_USER@\$EC2_HOST:~/"

            // SSH into the EC2 instance and deploy the application
            sh "ssh -i \$WORKSPACE/zebbara-ssh-aws.pem -v \$EC2_USER@\$EC2_HOST 'nohup java -jar ~/\$JAR_FILE_NAME > app.log 2>&1 &'"
        } 
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
