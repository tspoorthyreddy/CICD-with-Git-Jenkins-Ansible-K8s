# Deploy Artifact on a Tomcat Server

![Screen Shot 2024-02-20 at 4 47 26 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/964358cc-edf1-49f7-a0f4-93fa4a275007)

Setup Tomcat Server (tomcat version 9.0)
1. Setup a Linux EC2 Instance
2. Install Java
3. Configure Tomcat
4. Start Tomcat Server
5. Access Web UI on port 8080

Create EC2 instance with Amazon Linux 2

Download the tomcat package from https://tomcat.apache.org/download-90.cgi
```
amazon-linux-extras install java-openjdk11
java -version
cd /opt
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.86/bin/apache-tomcat-9.0.86.tar.gz
tar -xvzf apache-tomcat-9.0.86.tar.gz
mv apache-tomcat-9.0.86 tomcat
cd /opt/tomcat/bin
./startup.sh
```
In the browser <server public ip>:8080
Go to manager, if you see 403 error 

```
cd ../..
cd tomcat/
find / -name context.xml

#comment these lines in the below files
 <!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
  allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->

vi /opt/tomcat/webapps/host-manager/META-INF/context.xml
vi /opt/tomcat/webapps/manager/META-INF/context.xml
vi /opt/tomcat/webapps/docs/META-INF/context.xml
```
