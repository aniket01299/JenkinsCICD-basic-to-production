# JenkinsCICD-basic-to-production 

# 🚀 Django Notes App - End-to-End CI/CD Pipeline

## 📌 Project Overview

This project demonstrates a complete DevOps CI/CD implementation for a Django Notes Application using Jenkins, Docker, DockerHub, GitHub Webhooks, and AWS EC2.

The pipeline automates the entire software delivery lifecycle from code commit to deployment.

Whenever code is pushed to GitHub, Jenkins automatically:

✅ Clones the latest source code

✅ Builds a Docker image

✅ Pushes the image to DockerHub

✅ Deploys the latest version on AWS EC2

---

## 🏗️ Architecture

```text
Developer
    │
    ▼
 GitHub Repository
    │
    ▼
 GitHub Webhook
    │
    ▼
 Jenkins Pipeline
    │
 ┌──┴─────────────┐
 ▼                ▼
Docker Build   DockerHub Push
    │
    ▼
Docker Compose Deployment
    │
    ▼
Django Notes App on AWS EC2
```

---

## 🛠️ Tech Stack

| Technology        | Purpose                    |
| ----------------- | -------------------------- |
| 🐍 Django         | Web Application            |
| 🐳 Docker         | Containerization           |
| 📦 Docker Compose | Multi-container Deployment |
| 🔄 Jenkins        | CI/CD Automation           |
| 🐙 GitHub         | Source Code Management     |
| 📚 DockerHub      | Image Registry             |
| ☁️ AWS EC2        | Hosting Infrastructure     |
| 🐧 Linux          | Operating System           |

---

# 🔧 Jenkins Setup

## Step 1️⃣ Install Java

```bash
sudo apt update
sudo apt install openjdk-21-jdk -y
```

Verify:

```bash
java -version
```

---

## Step 2️⃣ Install Jenkins

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
```

Start Jenkins:

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Check status:

```bash
sudo systemctl status jenkins
```

---

## Step 3️⃣ Configure AWS Security Group

Allow inbound traffic:

| Port | Purpose         |
| ---- | --------------- |
| 22   | SSH             |
| 8080 | Jenkins         |
| 8000 | Django App      |
| 80   | HTTP (Optional) |

---

## Step 4️⃣ Unlock Jenkins

Open:

```text
http://<EC2-PUBLIC-IP>:8080
```

Get password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Install suggested plugins.

---

## Step 5️⃣ Install Jenkins Plugins

Install:

* Git Plugin
* Docker Pipeline
* Credentials Binding
* GitHub Integration
* Pipeline Plugin
* SSH Agent Plugin

Restart Jenkins.

---

## Step 6️⃣ Configure Jenkins Agent

Navigate:

```text
Manage Jenkins
→ Nodes
→ New Node
```

Create:

```text
Node Name: Agent-vinod
Type: Permanent Agent
```

Configuration:

```text
Remote Root Directory:
/home/ubuntu

Labels:
vinod

Executors:
1
```

Connect Agent:

```bash
curl -O http://<JENKINS-IP>:8080/jnlpJars/agent.jar

java -jar agent.jar \
-url http://<JENKINS-IP>:8080/ \
-secret <SECRET> \
-name Agent-vinod \
-workDir "/home/ubuntu"
```

Verify Agent Status: 🟢 Online

---

## Step 7️⃣ Configure DockerHub Credentials

Navigate:

```text
Manage Jenkins
→ Credentials
→ Global Credentials
```

Create:

```text
Kind: Username with Password
ID: dockerHubCred
```

Store DockerHub username and password/token.

---

## Step 8️⃣ Create Jenkins Pipeline

Create New Item → Pipeline

Configure:

```text
SCM: Git

Repository:
https://github.com/<username>/django-notes-app.git

Branch:
main
```

Save pipeline.

---

## Step 9️⃣ Configure GitHub Webhook

Repository:

```text
Settings
→ Webhooks
→ Add Webhook
```

Payload URL:

```text
http://<JENKINS-PUBLIC-IP>:8080/github-webhook/
```

Content Type:

```text
application/json
```

Events:

```text
Just the push event
```

Save Webhook.

---

## Step 🔟 Enable Build Trigger

Job Configuration:

```text
Build Triggers

☑ GitHub hook trigger for GITScm polling
```

Save.

---

# 🚀 Application Deployment

## Clone Repository

```bash
git clone https://github.com/aniket01299/django-notes-app.git
cd django-notes-app
```

---

## Build Docker Image

```bash
docker build -t notes-app:latest .
```

---

## Run Containers

```bash
docker-compose up -d
```

Check running containers:

```bash
docker ps
```

---

## Stop Containers

```bash
docker-compose down
```

---

# 🔄 CI/CD Workflow

### 1️⃣ Developer Pushes Code

```bash
git add .
git commit -m "Feature Update"
git push origin main
```

### 2️⃣ GitHub Webhook Triggered

Webhook sends notification to Jenkins.

### 3️⃣ Jenkins Starts Pipeline

Pipeline executes automatically.

### 4️⃣ Docker Image Build

```bash
docker build -t notes-app:latest .
```

### 5️⃣ Push Image to DockerHub

```bash
docker push <dockerhub-username>/notes-app:latest
```

### 6️⃣ Deploy Latest Version

```bash
docker-compose down
docker-compose up -d
```

### 7️⃣ Application Live 🎉

Users can access the latest deployed application.

---

# 📂 Project Structure

```text
django-notes-app/
│
├── Dockerfile
├── docker-compose.yml
├── Jenkinsfile
├── requirements.txt
├── manage.py
│
├── notes/
├── users/
├── templates/
└── static/
```

---

# 📸 Screenshots

Add screenshots for:

📷 Jenkins Dashboard

📷 Successful Pipeline Run

📷 GitHub Webhook Delivery

📷 Docker Containers Running

📷 AWS EC2 Instance

📷 Django Application UI

---

# 🎯 Key Learning Outcomes

✅ Jenkins Pipeline Creation

✅ Docker Image Management

✅ DockerHub Integration

✅ GitHub Webhooks

✅ Jenkins Agent Configuration

✅ AWS EC2 Administration

✅ Automated CI/CD Deployments

✅ DevOps Best Practices

---

# 👨‍💻 Author

### Aniket Siraskar

🚀 DevOps Engineer

☁️ AWS | Jenkins | Docker | Linux | CI/CD | GitHub Actions

