# Docker in CICD

![Screen Shot 2024-02-20 at 4 49 35 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/e92f1210-93da-435a-8db2-273e08e4c88b)

Setup Docker Host (Docker version 20.10)
1. Setup a Linux EC2 Instance
2. Install docker
3. Start docker services
4. Basic docker commands

```
yum install docker -y
docker --version
service docker start
service docker status
```

Pull tomcat image from dockerhub
create container using that image

```
docker pull tomcat
docker run -d --name tomcat-container -p 8081:8080 tomcat
docker ps -a
```

On browser <server pub IP>:8081
If you are getting HTTP Status 404 - Not Found error
```
docker exec -it tomcat-container /bin/bash     # login to the container
cd webapps.dist/
cp -R * ../webapps                             # copy contents from webapps.dist to webapps
```

![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/fc9b516e-b18c-4a46-81fa-ebfb206dbd85)

Write a custom Dockerfile of Tomcat image with resolved 404 issue

```
vi Dockerfile

From tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
```
Using the image create a container
```
docker build -t mytomcat .
docker images
docker run -d --name mytomcat-server -p 8083:8080 mytomcat
docker stop mytomcat-server
```
Integrate docker with Jenkins
1. Create a dockeradmin user
2. Install "Publish Over SSH" plugin
3. Add Dockerhost to Jenkins "configure systems"

```
cat /etc/group
useradd dockeradmin
passwd dockeradmin
id dockeradmin
usermod -aG docker dockeradmin
id dockeradmin
su - dockeradmin
```
Dashboard > Manage Jenkins > Systems
Add SSH servers
Name: dockerhost
Hostname: docker server private ip
Username : dockerhost
check use password authentication
give the password
Test connection
save

Create a job to pull the code from git and build it with maven and push the artifact to dockerhost

```
cd /opt
mkdir docker
chown -R dockeradmin:dockeradmin docker
mv /root/Dockerfile /opt/docker
chown -R dockeradmin:dockeradmin Dockerfile
```
Now in the build see to that the war file is deployed in /opt/docker
Now we need to modify the Dockerfile to automatically pick the war file from the dockerhost into the container
befor
 ```
vi Dockerfile

From tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
COPY ./*.war /usr/local/tomcat/webapps

```

Now modify the job to automatically create an image and container
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/44c9b81a-d018-4c16-8af2-18c9799becf7)
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/1e9c2da3-1189-48b9-994c-65853a76d64c)
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/1a0828ff-77dc-40ac-989c-be1c8a2c5dba)
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/3036980d-3a78-44d8-83d5-7b8a935848cf)


