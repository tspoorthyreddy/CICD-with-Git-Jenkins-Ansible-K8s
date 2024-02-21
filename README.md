# CICD using Git, Jenkins & Maven
Create EC2 with Amazon Linux 2 

ssh into the instance
From https://www.jenkins.io/download/ copy the first 2 steps of repo like below

```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

Install epel, jdk, jenkins

```
amazon-linux-extras install epel
java -version
amazon-linux-extras install java-openjdk11
yum install jenkins
service jenkins status
service jenkins start
```

Integrate GitHub with Jenkins
1. Install Git on Jenkins Instance
2. Install GitHub Plugin on Jenkins GUI
3. Configure Git on Jenkins GUI

```
yum install git
git --version
```

![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/480b1fd6-f2b9-4e54-876d-d90712ce7b0c)
