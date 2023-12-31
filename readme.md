﻿
# Project Overview
Implement a continuous integration and continuous deployment (CI/CD) workflow by automating the deployment of a Spring Boot application to an AWS EC2 instance through Jenkins pipelines.

![architecture](images/architecture.png)

## Key Features:

Build and package your Spring Boot app with Maven.
Securely deploy it to EC2 instances via SSH using Jenkins.
Achieve efficient, automated deployments and monitor success/failures.

## Getting Started:

Follow our comprehensive guide to configure Jenkins, set up your EC2 instances, and start deploying your Spring Boot apps quickly and reliably.


### Configure Jenkins:

---
Set up Jenkins on a server or cloud instance.
Install necessary plugins, including the SSH Agent plugin and any other required plugins.
Ensure Jenkins has the necessary permissions to access your source code repository (e.g., GitHub, Bitbucket, GitLab).

### Create Jenkins Credentials:

---

Create SSH key credentials in Jenkins to securely store the SSH private key that will be used to connect to the EC2 instance.


### Configure EC2 Instance:

---
Set up an EC2 instance on AWS where you want to deploy your Spring Boot application.
Ensure that your EC2 instance has the required dependencies and runtime environment for your Spring Boot application.
Add your Jenkins server's public SSH key to the ~/.ssh/authorized_keys file on your EC2 instance to allow Jenkins to SSH into the instance.


### Create a Jenkins Pipeline:

---
Create a Jenkins pipeline job using a Jenkinsfile or through the Jenkins user interface.
Define the pipeline stages, including the "Build" and "Deploy" stages.

### Configure Jenkins Pipeline Environment:

---
Define environment variables in your Jenkinsfile for the EC2 instance details, such as EC2_HOST, EC2_USER, SSH_CREDENTIALS_ID, and JAR_FILE_NAME.
Use the withCredentials step to securely access the SSH private key.

For example:

````bash
   EC2_HOST = '54.89.35.174' // Update with your EC2 instance's IP address
   EC2_USER = 'ec2-user'     // Update with your EC2 instance's SSH username
   JAR_FILE_NAME = 'myapp-0.0.1-SNAPSHOT.jar' 
````


### Build Stage:

---
In the "Build" stage, use the sh step to build your Spring Boot application using Maven.

For example: 

````bash
sh 'mvn clean package'
````


### Deploy Stage:

---
In the "Deploy" stage, use the withCredentials step to securely access the SSH private key.
Use the sh step to perform the following tasks:
Copy the JAR file from the Jenkins workspace to the EC2 instance using scp.
SSH into the EC2 instance and start the Spring Boot application in the background.

For example:

````bash
     // Add the host key to known_hosts file in Jenkins
    sh "ssh-keyscan -H $EC2_HOST >> ~/.ssh/known_hosts"
    
    // Copy the JAR file to the EC2 instance using scp with -i
    sh "scp -i \$WORKSPACE/zebbara-abdessamad-ssh.pem -v target/\$JAR_FILE_NAME \$EC2_USER@\$EC2_HOST:~/"
    
     // SSH into the EC2 instance and install Java 11 from Amazon Linux 2 repositories
    sh "ssh -i \$WORKSPACE/zebbara-abdessamad-ssh.pem \$EC2_USER@\$EC2_HOST 'sudo  yum install -y maven'"
    
    // SSH into the EC2 instance and deploy the application
    sh "ssh -i \$WORKSPACE/zebbara-abdessamad-ssh.pem \$EC2_USER@\$EC2_HOST -v 'java -jar ~/myapp-0.0.1-SNAPSHOT.jar > app.log 2>&1 &'"
````

### Post-Build Actions:

---
Define post-build actions, such as echoing a success or failure message, to provide feedback on the deployment status.

### Run the Jenkins Job:

---
Trigger the Jenkins job manually or configure it to be triggered automatically upon code changes or other events.

### Troubleshoot and Debug:

---
Monitor the Jenkins console output and logs for any errors or issues during the build and deployment stages.
Address any errors or configuration issues that may arise.

### Verify Deployment:

---
Check the EC2 instance to ensure that the Spring Boot application is running as expected.
Verify that the application is accessible via its public IP or domain name.


## License

The code and documentation in this repository are provided under the following license:

[MIT License](https://opensource.org/licenses/MIT)

© 2023 Zebbara Abdessamad
