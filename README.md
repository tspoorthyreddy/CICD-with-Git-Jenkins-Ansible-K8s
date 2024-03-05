# K8s in CICD

![Screen Shot 2024-02-20 at 4 53 00 PM](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/dec5740a-34d7-4705-b6ae-efb638f85571)

Create EC2 instance with amazon 2 linux 

Update awscli to be able to install ekscli (follow https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
```
sudo yum remove awscli
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
cd ~
ll -a
vi .bash_profile

PATH=$PATH:$HOME/bin:/usr/local/bin

source .bash_profile
echo $PATH
```

Install kubectl (follow https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
```
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.29.0/2024-01-04/bin/linux/amd64/kubectl
ll
chmod +x kubectl
ll
mv kubectl /usr/local/bin/
kubectl version
```

Install eksctl
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
cd /tmp
mv eksctl /usr/local/bin
eksctl version
```

IAM role to k8s server
Give EC2fullaccess, IAMfullaccess, cloudformationfullaccess, administrator access
Attach this role to EKS server

Create your cluster and nodes
```
eksctl create cluster --name regapp-cluster  \
--region us-east-1 \
--node-type t2.small \
--zones us-east-1a,us-east-1b

```

Write Deployment and service manifest files to create pods and service
```
vi regapp-deployment.yml
vi regapp-service.yml
```

Integrate kubernetes with Ansible

--> On K8s Server

1.Create ansadmin user
```
useradd ansadmin
passwd ansadmin
```
2.Add user to sudoers file
```
visudo
```
add the line ansadmin ALL=(ALL) NOPASSWD: ALL
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/fd6614e8-d185-4b2d-a7bc-a716b194704d)

3. Enable password based login
```
vi /etc/ssh/sshd_config
```
unhash Password yes and hash password no
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/0e12f21f-d9c5-45f6-b2c1-0d69b80ba12b)
```
service sshd reload
```

--> On Ansible Server

1.Add to hosts file
```
vi /etc/ansible/hosts
```
Add K8s server private IP and ansible private IP .save and exit

2.Copy ssh keys
```
su - ansadmin
ll -a
ssh-copy-id root@<kubernetes pri ip>
```
3.Test the connection
```
ansible all -m ping                 # to check the connection with all the hosts that the ansible server is connected to
ansible all -m command -a uptime    # to check uptime of all the servers connected to ansible
```

On ansible server
```
cd /opt/docker
mv regapp.yml create_image_regapp.yml
mv deploy_regapp.yml docker_deployment.yml
```
Write kube_deploynservice.yml playbook


On jenkins UI
Create a job to pull code from git , build the code , push the artifact to ansible and build image on ansible and push it to dockerhub
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/bd99baae-9b4c-4eae-82c7-237d8c232961)
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/37849cef-de69-4ad9-882d-18f2e574401f)
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/32b5b2c5-95dc-4031-9058-ecb601a08ae4)
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/2c7b8e8b-fbee-4f26-98ae-534986629277)
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/2a816a85-ba19-4278-85c1-338997d5c362)

create a new job to pull an image from dockerhub and deploy on kubernetes
![image](https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s/assets/93954534/20d64caa-8d91-498d-b705-331f693c012f)


To delete cluster
```
eksctl delete cluster regapp-cluster --region us-east-1
```


