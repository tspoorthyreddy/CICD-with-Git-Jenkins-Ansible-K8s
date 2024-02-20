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


