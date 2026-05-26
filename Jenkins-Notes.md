
# Jenkins Notes

## What is Jenkins?

Jenkins is an open-source CI/CD automation server used to automate:

- Build
- Test
- Deploy
- Infrastructure automation

---

# Jenkins Architecture

Developer
↓
GitHub
↓
Jenkins
↓
Build/Test
↓
Docker/Terraform/Kubernetes
↓
AWS Deployment

---

# Jenkins Features

- Continuous Integration (CI)
- Continuous Delivery (CD)
- Pipeline as Code
- Plugin Support
- Distributed Builds
- Docker Integration
- Kubernetes Integration
- AWS Integration

---

# Jenkins Installation (Ubuntu)

## Install Java

```bash
sudo apt update
sudo apt install openjdk-21-jdk -y
java -version
```

---

## Install Jenkins

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
```

---

## Start Jenkins

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

---

# Jenkins Default Port

```text
8080
```

Access Jenkins:

```text
http://<server-ip>:8080
```

---

# Get Jenkins Admin Password

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

# Important Jenkins Directories

| Path | Purpose |
|------|----------|
| /var/lib/jenkins | Jenkins Home |
| /var/log/jenkins | Logs |
| /etc/default/jenkins | Config |
| /var/cache/jenkins | Cache |

---

# Jenkins Pipeline Types

## 1. Freestyle Project

Simple GUI-based jobs.

---

## 2. Pipeline Project

Pipeline written as code using Jenkinsfile.

Example:

```groovy
pipeline {
  agent any

  stages {

    stage('Build') {
      steps {
        echo 'Building application...'
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying application...'
      }
    }
  }
}
```

---

# Declarative Pipeline Structure

```groovy
pipeline {

  agent any

  environment {
    ENV = 'dev'
  }

  stages {

    stage('Stage Name') {
      steps {
        echo 'Hello Jenkins'
      }
    }
  }

  post {
    success {
      echo 'Pipeline Success'
    }

    failure {
      echo 'Pipeline Failed'
    }
  }
}
```

---

# Important Jenkins Plugins

| Plugin | Purpose |
|--------|----------|
| Git Plugin | Git Integration |
| Docker Plugin | Docker Integration |
| Pipeline Plugin | CI/CD Pipelines |
| Blue Ocean | UI |
| AWS Credentials Plugin | AWS Access |
| Amazon ECS Plugin | ECS Agents |

---

# Jenkins With Docker

## Build Docker Image

```bash
docker build -t app:v1 .
```

---

## Run Docker Container

```bash
docker run -d -p 8080:80 app:v1
```

---

# Jenkins With Terraform

## Terraform Commands

```bash
terraform init
terraform plan
terraform apply
terraform destroy
```

---

# Jenkins ECS Dynamic Agents

## Workflow

Jenkins Master
↓
ECS Cloud
↓
Dynamic ECS Agent
↓
Pipeline Execution

---

# ECS Agent Requirements

Custom Docker image should contain:

- Terraform
- AWS CLI
- Git
- Docker CLI (optional)

---

# Dockerfile For ECS Jenkins Agent

```dockerfile
FROM jenkins/inbound-agent:latest

USER root

RUN apt-get update && apt-get install -y \
    wget \
    unzip \
    curl \
    gnupg \
    lsb-release \
    awscli

RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor \
    -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

RUN echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com bookworm main" \
> /etc/apt/sources.list.d/hashicorp.list

RUN apt-get update && apt-get install -y terraform

USER jenkins
```

---

# Common Jenkins Commands

## Restart Jenkins

```bash
sudo systemctl restart jenkins
```

---

## Check Jenkins Status

```bash
sudo systemctl status jenkins
```

---

## View Jenkins Logs

```bash
sudo journalctl -u jenkins -f
```

---

# Common Jenkins Errors

## terraform: not found

Cause:

```text
Terraform not installed in Jenkins agent container.
```

Fix:

```text
Use custom Docker image with Terraform installed.
```

---

## aws: not found

Cause:

```text
AWS CLI missing in container.
```

Fix:

```text
Install AWS CLI in Jenkins agent image.
```

---

## Could not find credentials

Cause:

```text
Wrong Jenkins credentials ID.
```

Fix:

```text
Create correct credentials in Jenkins.
```

---

# Best Practices

- Use Jenkinsfile
- Store code in GitHub
- Use credentials securely
- Use Docker agents
- Use dynamic ECS agents
- Use separate DEV/PROD pipelines
- Use Terraform state locking

---

# Useful Jenkins Files

| File | Purpose |
|------|----------|
| Jenkinsfile | Pipeline code |
| Dockerfile | Build agent image |
| README.md | Documentation |

---

# Author

Sameer Hadke
DevOps & Cloud Learning Notes