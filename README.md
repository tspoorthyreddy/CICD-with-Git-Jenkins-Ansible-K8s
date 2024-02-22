# Ansible in CICD

![WhatsApp Image 2024-02-22 at 4 13 12 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/b241c7f6-f570-4e27-b1c3-04c6e2677bfe)

1. Setup EC2 Instance
2. Setup hostname
3. Create ansadmin user
   ```
   useradd ansadmin
   passwd ansadmin
   ```
4. Add user to sudoers file
  visudo
add the line ansadmin ALL=(ALL) NOPASSWD: ALL
![Screen Shot 2024-02-22 at 4 47 21 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/3fae2c36-48e5-42f7-b3c3-74a675f02a9e)


6. Generate ssh keys
7. Enable password based login
8. Install ansible
