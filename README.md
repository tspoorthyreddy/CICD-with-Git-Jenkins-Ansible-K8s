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
13. Configure ansible server(Create ansadmin user , add it to sudoers file.Generate ssh keys, generate password based login) and install ansible.
   
14.Configure docker host so it can be managed by ansible servera(do the same process of creating user, add to sudoers and enable password based login on docker host).
15. Update the hosts file on ansible and copy the ssh keys(On ansible server add docker ip and ansible ip in the hosts file and copy ssh keys from ansible to docker and ansible to ansible).
16. Integrate ansible with jenkins.
17. Create ansible playbooks to create an image using the artifact and to push the artifact to docker hub and also trigger docker host to pull the image from docker hub and create a container.
-------------------------------------------------------------------------------------------------------------------------------------------------------
18. I create another EC2 instance for K8s
19. Install awscli,ekscli,kubectl,create IAM role for your ec2 and finally create your cluster and nodes
20. On k8s server write deployment and service manifest files
21. Integrate Kubernetes with ansible(create ansible user on k8s, add it to sudoers file, enable password based login and on ansible server add k8s ip to hosts fils, copy ssh keys onto root user from ansible and test the connection)
22. On ansible server write deploy and service.yml playbook which triggers the deployment and service manifest files on k8s server
23. Finally integrate jenkins create CICD pipelines which will pick the code from git build using maven , push the artifact to ansible where image will be created using this artifact and the image will be pushed to dockerhub, and ansible will trigger kubernetes to pull the image and deploy pods and service
