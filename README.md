# Jenkins CI/CD with GitHub Integration (Deploying Node.js To-Do Application)

## Overview
This project demonstrates how to set up a CI/CD pipeline using Jenkins integrated with GitHub for continuous integration and deployment of a Node.js To-Do application on AWS EC2 using Docker. The pipeline automates the build, test, and deployment processes, ensuring that changes pushed to GitHub are automatically reflected in the deployed application.

## Resources

- **Video Tutorial by Shubham Londhe: https://www.youtube.com/live/nplH3BzKHPk?si=FWK-NVSa8t-rIYOW
- **Hashnode Blog: https://amitabhdevops.hashnode.dev/jenkins-cicd-with-github-integration

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

With this Jenkins CI/CD pipeline, you have automated the process of building, testing, and deploying your Node.js application on AWS EC2. The integration with GitHub ensures that any changes pushed to the repository are automatically deployed, fostering continuous development and deployment.

Feel free to explore the project and implement similar setups for your own applications!

## Output of My Project :

1 ) Making changes in Github repo :
         ![My Image](https://github.com/user-attachments/assets/006cef11-809f-4147-9710-488c9f054ad3)

2 ) Checking the Contineous Integration of code on Jenkins :
         ![My Image](https://github.com/user-attachments/assets/d2c284d5-3709-4d4c-b068-9955218295ec)

3 ) Changes is Successfully applied to Project using Github push :
         ![My Image](https://github.com/user-attachments/assets/0ed28d0b-5f6e-421a-a80e-9d17855d346d)
         
4 ) Checking the Output :
         ![My Image](https://github.com/user-attachments/assets/562d47e6-3e68-43ab-9b7c-194cbcdd0c5b)
         
