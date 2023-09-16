pipeline {
    agent any
      tools {
        // Specify the name of the Maven installation defined in Jenkins
        maven 'Maven 3.6.3'
    }

    environment {
        EC2_HOST = '3.81.236.160' // Update with your EC2 instance's IP address
        EC2_USER = 'ec2-user'     // Update with your EC2 instance's SSH username
        SSH_CREDENTIALS_ID = credentials('3.81.236.160') // Update with your SSH key credential ID
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
            def remoteHost = '3.81.236.160'  // Update with your EC2 instance's IP address
            def remoteUser = 'ec2-user'      // Update with your EC2 instance's SSH username
            def privateKey = credentials('3.81.236.160')  // Use the ID of your SSH private key credential

            sh "scp -i ${privateKey} target/${jarFileName} ${remoteUser}@${remoteHost}:~/"

            // SSH into the remote server and start the application
            sh "ssh -i ${privateKey} ${remoteUser}@${remoteHost} 'nohup java -jar ~/${jarFileName} > app.log 2>&1 &'"
                
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
