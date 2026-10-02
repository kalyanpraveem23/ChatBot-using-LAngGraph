# 🤖 Agentic Chatbot — CI/CD Deployment with GitHub Actions on AWS

An **Agentic Chatbot application** deployed on **AWS EC2** using **Docker** and an automated **CI/CD pipeline with GitHub Actions**.

The deployment workflow builds the application into a Docker image, pushes the image to Docker Hub, and deploys the latest image to an AWS EC2 instance.

---

## 🚀 Deployment Architecture

```text
Developer
   │
   │ git push
   ▼
GitHub Repository
   │
   ▼
GitHub Actions
   │
   ├── Build Docker Image   
   │
   ├── Login to Docker Hub
   │
   ├── Push Docker Image
   │
   ▼
Docker Hub
   │
   │ Pull Image
   ▼
AWS EC2 (Ubuntu)
   │
   ├── Docker Container
   │
   └── Port 8501
   │
   ▼
Streamlit Agentic Chatbot
```

---

# 📌 Project Description

The deployment process consists of the following steps:

1. Build a Docker image from the source code.
2. Push the Docker image to Docker Hub.
3. Launch an AWS EC2 Ubuntu instance.
4. Configure the EC2 instance as a GitHub Actions self-hosted runner.
5. Pull the Docker image from Docker Hub.
6. Run the Docker container on the EC2 instance.
7. Access the Streamlit application through port **8501**.

---

# ☁️ AWS Setup

## 1. Login to AWS Console

Log in to the AWS Management Console.

Create or use an AWS account with permission to create and manage EC2 resources.

---

# 🔐 2. Create IAM User for Deployment

Create an IAM user for deployment.

### Required Policy

For the basic setup described in this project:

```text
AmazonEC2FullAccess
```

> ⚠️ **Security recommendation:** For production deployments, avoid using broad permissions such as `AmazonEC2FullAccess`. Use a least-privilege IAM policy containing only the permissions required by your deployment workflow.

Create an access key for the IAM user if your GitHub Actions workflow requires AWS API access.

---

# 🖥️ 3. Create EC2 Instance

Create an EC2 instance with:

* **Operating System:** Ubuntu
* **Instance Type:** Choose according to your application requirements
* **Storage:** Configure as required
* **Security Group:** Allow SSH and application traffic

### Required Ports

| Port | Purpose               |
| ---: | --------------------- |
|   22 | SSH                   |
| 8501 | Streamlit application |

For example:

```text
TCP 22    → SSH
TCP 8501  → Streamlit
```

> ⚠️ Restrict SSH access to your IP address instead of allowing `0.0.0.0/0` whenever possible.

---

# 🐳 4. Install Docker on EC2

Connect to the EC2 instance through SSH.

Update the package list:

```bash
sudo apt-get update -y
```

Optional:

```bash
sudo apt-get upgrade -y
```

## Install Docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

Add the Ubuntu user to the Docker group:

```bash
sudo usermod -aG docker ubuntu
```

Apply the group changes:

```bash
newgrp docker
```

Verify Docker:

```bash
docker --version
```

Test Docker:

```bash
docker run hello-world
```

---

# 🌐 5. Configure Streamlit Port

The application runs on:

```text
8501
```

Your Docker container should expose port `8501`.

Example:

```bash
docker run -d -p 8501:8501 agentic-chatbot
```

The application can then be accessed using:

```text
http://<EC2-PUBLIC-IP>:8501
```

---

# ⚙️ 6. C
