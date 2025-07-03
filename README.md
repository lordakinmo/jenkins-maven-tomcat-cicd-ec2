# Jenkins + Maven + Tomcat CI/CD Pipeline on AWS EC2

This project demonstrates a simple CI/CD pipeline using Jenkins, Maven, and Tomcat, deployed on an AWS EC2 instance.

Inspired by Ashfaque’s (https://github.com/Ashfaque-9x) DevOps project, this project includes my personalized modifications to the Jenkins-Maven-Tomcat CI/CD pipeline.

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

```

### Maven Installation and Configuration
```bash
# Download and install Maven
cd /opt
sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.10/binaries/apache-maven-3.9.10-bin.tar.gz
sudo tar -xzvf apache-maven-3.9.10-bin.tar.gz
sudo mv apache-maven-3.9.10 maven

# Setup environment variables
echo 'export M2_HOME=/opt/maven' >> ~/.bash_profile
echo 'export M2=/opt/maven/bin' >> ~/.bash_profile
echo 'export JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto.x86_64' >> ~/.bash_profile
echo 'export PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2' >> ~/.bash_profile
source ~/.bash_profile

# Verify Maven
mvn -v
```
### Tomcat Server Setup
```bash
# Install Java if not installed
sudo yum install java-17-amazon-corretto-devel -y
java -version

# Download and install Tomcat
cd /opt
sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.106/bin/apache-tomcat-9.0.106.tar.gz
sudo tar -xvzf apache-tomcat-9.0.106.tar.gz
sudo mv apache-tomcat-9.0.106 tomcat

# Find context.xml files to edit access permissions
find / -name context.xml

# Edit to comment out context.xml in host-manager and manager apps
sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
# Add <!-- at start and --> at end to comment out XML content

sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml

# Restart Tomcat to apply changes
cd /opt/tomcat/bin
sudo ./shutdown.sh
sudo ./startup.sh

# Configure Tomcat users (edit tomcat-users.xml)
sudo nano /opt/tomcat/conf/tomcat-users.xml

# Add user roles and users:
# <role rolename="manager-gui"/>
# <role rolename="manager-script"/>
# <role rolename="manager-jmx"/>
# <role rolename="manager-status"/>
# <user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
# <user username="deployer" password="deployer" roles="manager-script"/>
# <user username="tomcat" password="s3cret" roles="manager-gui"/>

# Start Tomcat server
cd /opt/tomcat/bin
sudo ./startup.sh
```
### Jenkins Job Configuration
```bash
# Create a new Jenkins job configured to poll SCM every minute with the schedule:
* * * * *
# Set up Maven build steps with the goal:
clean package
