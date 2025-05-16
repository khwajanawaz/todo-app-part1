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

### 2. Connect to EC2 Instance

```bash
ssh -i <your-key-file.pem> ubuntu@<instance-public-ip>

