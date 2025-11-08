# 🚀 RegApp CI/CD with Jenkins, Ansible, Docker, and EKS

This repository documents the setup of a complete CI/CD pipeline for the **RegApp** project — from code build and artifact transfer to automated Kubernetes deployment using Jenkins, Ansible, Docker, and Amazon EKS.

---

## 📺 YouTube Reference

Project tutorial followed:  
[Project YouTube Playlist](https://www.youtube.com/watch?v=5_s7EmZWz78&list=PLmSlOWkfkugnSv2TCC_-SiSpSzxFwqJVq)

---

## 🧰 Jenkins Installation

Official installation guide: [Jenkins on Linux](https://www.jenkins.io/doc/book/installing/linux/)

### Steps:

```bash
# Add Jenkins repository
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io-2023.key

# Add AWS Corretto Java repository
sudo rpm --import https://yum.corretto.aws/corretto.key
sudo curl -Lo /etc/yum.repos.d/corretto.repo https://yum.corretto.aws/corretto.repo

# Update the system
sudo dnf update -y

# Install Java 17 (Amazon Corretto)
sudo dnf install -y java-17-amazon-corretto java-17-amazon-corretto-devel
```

## 🔌 Installed Jenkins Plugins

- **Publish Over SSH**
- **GitHub Plugin(ON)**
- **GitHub Branch Source Plugin(OFF)**

---

## ⚙️ Jenkins Configuration

### 🧩 System Settings

- **Publish Over SSH** → Configured connection to _Ansible Server_
- **JDK** → `java17` | `/usr/lib/jvm/java-17-amazon-corretto.x86_64`
- **Maven** → `maven` | `/opt/maven`

---

## 🧱 Jenkins Jobs

### 🧰 RegApp_CI_Job

**Type:** Maven Project  
**Repository:** [GitHub - asafmis2/registration-app](https://github.com/asafmis2/registration-app)  
**Branch:** `main`  
**Trigger:** Poll SCM every minute (`* * * * *`)

#### 🔨 Build Step

```bash
mvn clean install
```

📦 Post-Build Actions

Trigger next project → RegApp_CD_job

Send artifacts over SSH

Source files: webapp/target/\*.war

Remove prefix: webapp/target

Remote directory: /opt/Docker

Exec command:
ansible-playbook /opt/Docker/creat_image_regapp.yml

🧩 RegApp_CD_job
Type: Freestyle Project
🧠 Post-Build Action
Send command via SSH to Ansible Server
ansible-playbook /opt/Docker/kube_deploy.yml

🧑‍💻 Ansible Server
Used to run automation playbooks that build Docker images and deploy them to Kubernetes.
Playbooks location: /opt/Docker/

## Example Playbook

- hosts: kubernetes
  become: yes

  tasks:

  - name: Deploy RegApp on Kubernetes
    command: kubectl apply -f regapp-deployment.yml

  - name: Create Service for RegApp
    command: kubectl apply -f regapp-service.yml

  - name: Restart deployment if image was updated
    command: kubectl rollout restart deployment.apps/virtualtechbox-regapp

☸️ EKS Cluster Setup
📘 Official documentation: AWS EKS User Guide

🧱 Create a new cluster
eksctl create cluster --name virtualtechbox-cluster --region eu-west-3 --node-type t3.small

📋 List existing clusters
aws cloudformation list-stacks --region eu-west-3

🧾 Summary
✅ Jenkins automates build and deployment using CI/CD.  
✅ Maven builds and packages the application.  
✅ Artifacts are sent via SSH to Ansible.  
✅ Ansible triggers Docker image creation and Kubernetes deployment on EKS.

This setup enables full automation from source code to running containers in the cloud.
