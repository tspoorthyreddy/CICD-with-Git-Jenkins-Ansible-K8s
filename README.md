# CICD using Git, Jenkins, Maven & Sonarqube

![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/860eeddd-4019-4ef5-88dd-8cf6d1cfbd60)

Create ubuntu EC2 with below userdata to setup Jenkins and Maven
```
#!/bin/bash
sudo apt update
sudo apt install openjdk-11-jdk -y
sudo apt install maven -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
  
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y
```
Access jenkins with server public ip:8080

Install Plugins Maven, Publish Over SSH on Jenkins

Integrate Sonarqube with Jenkins
1. Install Sonarqube on the Jenkins Instance
```
apt install unzip
adduser sonarqube
su - sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
```
2. Install "Sonarqube Scanner" Plugin on Jenkins GUI
  
3. Configure Sonarqube on Jenkins GUI

access sonarqube on server public ip:9000

Add sonarqube credentials in jenkins
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/ab4877f7-d00f-4129-bab2-28ccb59e340c)


