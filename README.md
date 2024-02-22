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

Create a docker image to install tomcat on centos

![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/bf855f05-124f-438a-b0e4-c229f6065d09)

