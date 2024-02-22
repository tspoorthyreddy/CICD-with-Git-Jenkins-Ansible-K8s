# CICD with Git Jenkins Ansible K8s
![Screen Shot 2024-02-20 at 4 53 00 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/083df7f7-c511-4207-9624-e275531833bd)

1. I create an Ec2 instance with user data to install Jenkins, maven

2. I create an EC2 instance install java and configure tomcat on it.
3. On Jenkins install a "Deploy to container" plugin and configure tomcat credentials on Jenkins
4. Deploy the application on tomcat server
  
5. I create another EC2 instance and install Docker on it
6. Write a custom Dockerfile which pulls Tomcat image from Dockerhub and resolves 404 issue
7. Using the image create a container
8. Create dockeradmin user
9. Install "Publish Over SSH" plugin on Jenkins and add dockerhost to jenkins
10. Create a job to pull the code from git and build it with maven and push the artifact to dockerhost which inturn will be copied to tomcat container
11. Automate the process of building the image and container

12. 
