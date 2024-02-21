# CICD using Git, Jenkins & Maven

![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/860eeddd-4019-4ef5-88dd-8cf6d1cfbd60)

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
systemctl enable jenkins
```

Integrate GitHub with Jenkins (Git version 2.39)
1. Install Git on Jenkins Instance
2. Install "GitHub" Plugin on Jenkins GUI
3. Configure Git on Jenkins GUI

```
yum install git
git --version
```

![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/480b1fd6-f2b9-4e54-876d-d90712ce7b0c)

go to jenkins UI Dashboard > Manage Jenkins > plugins > available plugins install Github plugin
copy the git path, go to jenkins UI Dashboard > Manage Jenkins > Tools Under Git Installations

![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/075fe103-0e8b-41c3-9f7e-320fd094fa66)

Integrate Maven with Jenkins (Maven version 3.8)
1. Setup Maven on Jenkins Server
2. Setup Environment Variables JAVA_HOME, M2, M2_HOME
3. Install Maven plugin
4. Configure Maven and Java

On the Jenkins Server
```
cd /opt
wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
ll
tar -xvzf apache-maven-3.9.6-bin.tar.gz
ll
mv apache-maven-3.9.6 maven     # renaming the file
ll
```

To set the environment variables

```
cd ~
find / -name java-11*  # Copy the jvm path
ll -a
vi .bash_profile

M2_HOME=/opt/maven
M2=/opt/maven/bin
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.22.0.7-1.amzn2.0.1.x86_64
PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2

```
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/f428606e-44f2-4907-88fb-6425cc267c8e)
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/198d8d8a-8742-4980-8896-1796a6997120)

```
source .bash_profile
echo $PATH
```

go to jenkins UI Dashboard > Manage Jenkins > plugins > available plugins install maven integration plugin
Manage Jenkins > Tools > Under JDK Installations and Maven installation give the home path

![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/4835e2a5-2721-4d54-bd13-df581d0e98fb)

Uncheck automatic maven installation 

![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/ca343272-57e7-4132-aa51-9bbfaeba53f2)

Create a Build job using maven project

Integrate Sonarqube with Jenkins (Sonarqube version )
1. Install Sonarqube on Jenkins Instance
2. Install "Sonarqube Scanner" Plugin on Jenkins GUI
3. Configure Sonarqube on Jenkins GUI

```
yum install unzip
adduser sonarqube
su - sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
```

Open port 9000 on jenkins server
access sonarqube on <server public ip>:9000
Install "Sonarqube Scanner" Plugin on Jenkins GUI
Add sonarqube credentials in jenkins
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/ab4877f7-d00f-4129-bab2-28ccb59e340c)


