#Jenkins CI/CD with GitHub Integration (Deploying Node.js To-Do Application)


#Overview

This project demonstrates how to set up a CI/CD pipeline using Jenkins integrated with GitHub for continuous integration and deployment of a Node.js To-Do application on AWS EC2 using Docker. The pipeline automates the build, test, and deployment processes, ensuring that changes pushed to GitHub are automatically reflected in the deployed application.

#Steps to Complete the Project

1. Create an EC2 Instance
Login to AWS Console and create a new EC2 instance with Ubuntu 24.04 LTS.
Configure Security Groups to allow inbound traffic on ports 22 (SSH), 8080 (Jenkins), and 8000 (Node.js application).
2. Connect to EC2 Instance
ssh -i <your-key-file.pem> ubuntu@<instance-public-ip>
3. Update the Instance
sudo apt update
4. Install Java
sudo apt install openjdk-17-jre
java -version
5. Install Jenkins
Install Dependencies:

sudo apt-get install -y ca-certificates curl gnupg
Add Jenkins GPG Key:

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
Add Jenkins Repository:

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
Update Package List:

sudo apt-get update
Install Jenkins:

sudo apt-get install jenkins
Enable and Start Jenkins:

sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
6. Access Jenkins
Open Jenkins: http://<instance-public-ip>:8080

Unlock Jenkins: Retrieve the admin password:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Install Suggested Plugins and create an admin user.

7. Create a New Jenkins Project
Create New Item: Name your project (e.g., NodeJS To-Do App), select Freestyle Project, and click OK.

Configure GitHub Integration:

Under Source Code Management, select Git and enter your GitHub repository URL.

Add SSH keys for GitHub authentication.

Generate SSH keys:

ssh-keygen
Add Public Key to GitHub: Go to GitHub Settings > SSH and GPG Keys > New SSH Key.

Add Private Key to Jenkins: Go to Source Code Management > Add Credentials > SSH Username with Private Key.

Configure Build Triggers:

Check GitHub hook trigger for GITScm polling.
8. Install Docker on EC2
sudo apt install docker.io
sudo usermod -a -G docker jenkins
sudo systemctl restart jenkins
9. Configure Build Steps
Add Build Step: Select Execute Shell and paste the following commands:

# Remove the existing container if it exists (ignore errors)
docker rm -f <desired_name_1> || true

# Build the Docker image
docker build . -t <desired_name_2>

# Run the container
docker run --rm -d --name <desired_name_1> -p 8000:8000 <desired_name_2>
10. Install GitHub Integration Plugin
Navigate to: Manage Jenkins > Manage Plugins > Available Plugins.
Install: GitHub Integration Plugin and restart Jenkins.
11. Configure GitHub Webhook
Go to GitHub Repository Settings > Webhooks.
Add Webhook:
Payload URL: http://<instance-public-ip>:8080/github-webhook/
Content Type: application/json
12. Open Application Port 8000
Modify Security Group of your EC2 instance to allow inbound traffic on port 8000.
13. Access the Deployed Application
Visit: http://<instance-public-ip>:8000
14. Continuous Deployment
Every push to your GitHub repository will trigger Jenkins to:

Pull the latest code.
Build a new Docker image.
Remove old containers.
Deploy the updated application.
