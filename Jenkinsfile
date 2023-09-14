pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/yourusername/your-repo.git']]])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package' # or your build command
            }
        }
        stage('Deploy') {
            steps {
                withAWS(region: 'us-east-1', credentials: 'aws-credentials-id') {
                    sh 'aws s3 cp target/my-app.jar s3://your-s3-bucket/'
                    sh 'aws ssm create-document --name "my-app-deployment" --document-type "Command" --content file://deployment-script.json'
                    sh 'aws ssm create-association --name "my-app-deployment-association" --targets "Key=instanceids,Values=your-ec2-instance-id"'
                }
            }
        }
    }
}
