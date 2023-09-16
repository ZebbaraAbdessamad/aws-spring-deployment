pipeline {
    agent any
      tools {
        // Specify the name of the Maven installation defined in Jenkins
        maven 'Maven 3.9.4'
    }

    environment {
        EC2_HOST = '54.88.11.100' // Update with your EC2 instance's IP address
        EC2_USER = 'ec2-user'     // Update with your EC2 instance's SSH username
        SSH_CREDENTIALS_ID = '54.88.11.100' // Update with your SSH key credential ID
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
                // Deploy your Spring Boot application to the EC2 instance
                script {
                    def remote = [:]
                    remote.name = 'ec2-instance'
                    remote.host = EC2_HOST
                    remote.user = EC2_USER
                    remote.identityFile = credentials(SSH_CREDENTIALS_ID)

                    remote.allowAnyHosts = true

                    // Copy the JAR/WAR file to the EC2 instance
                    remote.command = "scp -i ${remote.identityFile} target/*.jar ${remote.user}@${remote.host}:~/"
                    sshPut remote: remote

                    // SSH into the EC2 instance and deploy the application
                    remote.command = "ssh -i ${remote.identityFile} ${remote.user}@${remote.host} 'nohup java -jar ~/*.jar > app.log 2>&1 &'"
                    sshCommand remote: remote
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
