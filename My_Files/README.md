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
