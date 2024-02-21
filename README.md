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

cd bin
./shutdown.sh
./startup.sh
cd ..
cd conf
vi tomcat-users.xml
```
Update user's information in the tomcat-users.xml file
Place the below line at the end of the file before </tomcat-users>
```
 <role rolename="manager-gui"/>
 <role rolename="manager-script"/>
 <role rolename="manager-jmx"/>
 <role rolename="manager-status"/>
 <user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
 <user username="deployer" password="deployer" roles="manager-script"/>
 <user username="tomcat" password="s3cret" roles="manager-gui"/>
```

create link files for tomcat startup.sh and shutdown.sh
```
ln -s /opt/tomcat/bin/startup.sh /usr/local/bin/tomcatup
ln -s /opt/tomcat/bin/shutdown.sh /usr/local/bin/tomcatdown
tomcatup
```
check if echo $PATH has /usr/local/bin in its path if not 
```
cd ~
ll -a
vi .bash_profile

PATH=$PATH:$HOME/bin:/usr/local/bin     # add /usr/local/bin to the path line save and exit the file

source .bash_profile
echo $PATH
tomcatdown
tomcatup
```

Integrate Tomcat with Jenkins
1. Install "Deploy to container" plugin
2. On Jenkins Configure tomcat developer credentials 

