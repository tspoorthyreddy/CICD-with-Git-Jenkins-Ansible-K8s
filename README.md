# CICD with Git Jenkins Ansible K8s
![Screen Shot 2024-02-20 at 4 53 00 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/083df7f7-c511-4207-9624-e275531833bd)

1. I create an Ec2 instance with user data to install Jenkins, maven
-------------------------------------------------------------------------------------------------------------------------------------------------------
2. I create an EC2 instance install java and configure tomcat on it.
3. On Jenkins install a "Deploy to container", "Maven" plugin and configure tomcat credentials on Jenkins
4. Deploy the application on tomcat server
-------------------------------------------------------------------------------------------------------------------------------------------------------
5. I create another EC2 instance and install Docker on it
6. Write a custom Dockerfile which pulls Tomcat image from Dockerhub and resolves 404 issue
7. Using the image create a container
8. Create dockeradmin user
9. Install "Publish Over SSH" plugin on Jenkins and add dockerhost to jenkins
10. Create a job to pull the code from git and build it with maven and push the artifact to dockerhost which inturn will be copied to tomcat container
11. Automate the process of building the image and container
-------------------------------------------------------------------------------------------------------------------------------------------------------
12. I create another EC2 instance for Ansible
13. Configure ansible server(Create ansadmin user , add it to sudoers file.Generate ssh keys, generate password based login) and install ansible
15.Configure docker host so it can be managed by ansible servera(do the same process of creating user, add to sudoers and enable password based login on docker host)
16. Update the hosts file on ansible and copy the ssh keys(On ansible server add docker ip and ansible ip in the hosts file and copy ssh keys from ansible to docker and ansible to ansible)
17. Integrate ansible with jenkins
18. Create ansible playbooks to create an image using the artifact and to push the artifact to docker hub and also trigger docker host to pull the image from docker hub and create a container
