# Project Title (e.g., Jenkins CI/CD with GitHub Integration)

## Overview
This project demonstrates how to set up a CI/CD pipeline using Jenkins integrated with GitHub for continuous integration and deployment of a Node.js To-Do application on AWS EC2 using Docker. The pipeline automates the build, test, and deployment processes, ensuring that changes pushed to GitHub are automatically reflected in the deployed application.

## Resources

- **Video Tutorial by [Author Name]**: [YouTube Video](#)
- **Blog**: [Detailed Project Overview](#)

## Steps to Complete the Project

### 1. Create an EC2 Instance
1. **Login to AWS Console** and create a new EC2 instance with Ubuntu 24.04 LTS.
2. **Configure Security Groups** to allow inbound traffic on ports:
   - `22` (SSH)
   - `8080` (Jenkins)
   - `8000` (Node.js application)

## 2. Connect to EC2 Instance

```bash
ssh -i <your-key-file.pem> ubuntu@<instance-public-ip>
```

---

## 3. Update the Instance

```bash
sudo apt update
```

---

## 4. Update Package List

```bash
sudo apt-get update
```

## 5. Install Jenkins

```bash
sudo apt-get install jenkins
```

## 6. Enable and Start Jenkins

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

## 6. Access Jenkins

- **Open Jenkins**: `http://<instance-public-ip>:8080`

- **Unlock Jenkins**: Retrieve the admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

- **Install Suggested Plugins** and create an admin user.

---

## 7. Create a New Jenkins Project

### 1. Create New Item
- Name your project (e.g., `NodeJS To-Do App`)
- Select **Freestyle Project**
- Click **OK**

### 2. Configure GitHub Integration

- Under **Source Code Management**, select **Git** and enter your GitHub repository URL.
- **Add SSH keys** for GitHub authentication:

```bash
ssh-keygen
```

- **Add Public Key to GitHub**:  
  Go to GitHub → **Settings** → **SSH and GPG Keys** → **New SSH Key**

- **Add Private Key to Jenkins**:  
  Go to Jenkins → **Source Code Management** → **Add Credentials** → **SSH Username with Private Key**

### 3. Configure Build Triggers

- Check **GitHub hook trigger for GITScm polling**

---

## 8. Install Docker on EC2

```bash
sudo apt install docker.io
sudo usermod -a -G docker jenkins
sudo systemctl restart jenkins
```

---

## 9. Configure Build Steps

### 1. Add Build Step
- Select **Execute Shell**
- Paste the following commands:

```bash
# Remove the existing container if it exists (ignore errors)
docker rm -f <desired_name_1> || true

# Build the Docker image
docker build . -t <desired_name_2>

# Run the container
docker run --rm -d --name <desired_name_1> -p 8000:8000 <desired_name_2>
```

---

## 10. Install GitHub Integration Plugin

- Navigate to:  
  **Manage Jenkins > Manage Plugins > Available Plugins**

- Install:  
  **GitHub Integration Plugin** and restart Jenkins

---

## 11. Configure GitHub Webhook

- Go to GitHub Repository → **Settings** → **Webhooks**
- Add Webhook:
  - **Payload URL**: `http://<instance-public-ip>:8080/github-webhook/`
  - **Content Type**: `application/json`

---

## 12. Open Application Port 8000

- Modify the **Security Group** of your EC2 instance to allow inbound traffic on port **8000**

---

## 13. Access the Deployed Application

- Visit: `http://<instance-public-ip>:8000`

---

## 14. Continuous Deployment

Every push to your GitHub repository will trigger Jenkins to:

1. Pull the latest code
2. Build a new Docker image
3. Remove old containers
4. Deploy the updated application

---

## Conclusion

By following this guide, you've successfully set up a complete CI/CD pipeline using Jenkins, Docker, GitHub, and AWS EC2. This automation ensures that every code push triggers a sequence of actions — from pulling the latest code and building Docker images to deploying your updated Node.js application.

This setup not only improves your deployment speed and reliability but also encourages a DevOps culture of continuous integration and continuous deployment. It reduces manual intervention, mitigates deployment errors, and ensures consistent application delivery.

You can now confidently scale your application, integrate tests, or extend this pipeline with more sophisticated workflows and environments.




