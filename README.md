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
   ```
   visudo
   ```
add the line ansadmin ALL=(ALL) NOPASSWD: ALL
![Screen Shot 2024-02-22 at 4 47 21 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/3fae2c36-48e5-42f7-b3c3-74a675f02a9e)
5. Generate ssh keys
```
su - ansadmin
ssh-keygen
exit
```
6. Enable password based login
```
vi /etc/ssh/sshd_config
```
unhash Password yes and hash password no
![Screen Shot 2024-02-22 at 4 54 07 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/7c0cea95-d269-401c-9772-4c35491fa65c)
```
service sshd reload
```
7. Install ansible
```
amazon-linux-extras install ansible2
python --version
ansible --version
```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------

Manage DockerHost with Ansible

--> On Docker Host
1. Create ansadmin
```
useradd ansadmin
passwd ansadmin
```
2. Add ansadmin to sudoers files
 ```
   visudo
   ```
add the line ansadmin ALL=(ALL) NOPASSWD: ALL
![Screen Shot 2024-02-22 at 4 47 21 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/3fae2c36-48e5-42f7-b3c3-74a675f02a9e)
3. Enable password based login
```
vi /etc/ssh/sshd_config
```
unhash Password yes and hash password no
![Screen Shot 2024-02-22 at 4 54 07 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/7c0cea95-d269-401c-9772-4c35491fa65c)
```
service sshd reload
```
--> On Ansible Host
1. Add to hosts file
```
vi /etc/ansible/hosts
```
Delete everything already present in this file and add the docker server private IP to that file .save and exit
2. Copy ssh keys
```
su - ansadmin
ll -a
ssh-copy-id <docker server private ip>
```
4. Test the connection
```
ansible all -m ping                 # to check the connection with all the hosts that the ansible server is connected to
ansible all -m command -a uptime    # to check uptime of all the servers connected to ansible
```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------

Integrate Ansible with Jenkins

Create a job : give github details and maven clean install

Add SSH server
![Screen Shot 2024-02-22 at 5 26 38 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/40f63675-6bb8-441b-973d-5f21865a1515)

On ansible server
```
su - ansadmin
cd /opt
sudo mkdir docker
sudo chown -R ansadmin:ansadmin docker
```
Now when you run the job it will deploy artifact into Ansible
--------------------------------------------------------------------------------------------------------------------------------------------------------------------

Ansible Playbook to automatically create an image using the artifact
```
sudo vi /etc/ansible/hosts
```
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/eba42db8-e96a-4e75-9161-eaa764d6bc84)

```
ssh-copy-id <ansible server pri ip>
ansible all -a uptime
vi regapp.yml                         # write ansible playbook
ansible-playbook regapp.yml --check   # to check if the playbook is running fine
ansible-playbook regapp.yml
docker images
```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------

Automate Pushing created Docker image to docker hub

On ansible server
```
vi regapp.yml                         # modify ansible playbook
ansible-playbook regapp.yml --check   # to check if the playbook is running fine
ansible-playbook regapp.yml
```
On Jenkins server within the jon under ansible ssh server add the command to run ansible playbook under Exec Command
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/97497791-0cfe-4788-a6e3-7624760b64ec)

