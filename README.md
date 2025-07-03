# Jenkins + Maven + Tomcat CI/CD Pipeline on AWS EC2

This project demonstrates a simple CI/CD pipeline using Jenkins, Maven, and Tomcat, deployed on an AWS EC2 instance.

I followed a YouTube tutorial by [Ashfaque](https://github.com/Ashfaque-9x) and customized the setup with my own configurations and improvements.

---

## Features

- Jenkins installation and configuration on Amazon Linux 2 EC2 instance
- Maven setup for build automation
- Tomcat server for deploying Java web applications
- Poll SCM trigger for automated builds every minute
- Secure user setup in Tomcat
- Customized Jenkins jobs and pipeline scripts

---

## Installation & Setup Steps

### Jenkins Installation

```bash
# Update packages
sudo yum update -y

# Add Jenkins repo and import key
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

# Upgrade and install Java & Jenkins
sudo yum upgrade -y
sudo yum install java-17-amazon-corretto-devel -y
sudo yum install jenkins -y

# Enable and start Jenkins service
sudo systemctl enable jenkins
sudo systemctl start jenkins

# Verify Java and Jenkins status
java -version
javac -version
systemctl status jenkins
