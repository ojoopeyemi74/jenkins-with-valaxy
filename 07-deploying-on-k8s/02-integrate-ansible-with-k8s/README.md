#       we will be integrating K8s with Ansible 

# first step 
because we are managing our Clusters using the EC2 bootstrap server we will, so we will be integrating our ansbile with the EC2 bootstrap Instance 

- steps

- next: on the EC2 bootstrap instance ssh
we need to create a user named ansadmin
$ sudo su -
$ useradd ansadmin
$ passwd ansadmin

- next: we need to give sudo permission to the user ansadmin
$ visudo 
ansadmin ALL=(ALL)       ALL

- next: we need to give password Authentication 
$ sudo  nano /etc/ssh/sshd_config

PasswordAuthentication yes
#PermitEmptyPasswords no
#PasswordAuthentication no

$ service sshd reload 

# we need to integrate Ansible with EC2 bootstrap instance for k8S

- next: On the ansible host , we can create and inventory file
with the private IP of the bootstrap instance and also the ansible host

```
[kubernetes]
172.31.38.143

[ansible]
172.31.4.180

```

- next: we have to copy the ssh key from ansible-server to the bootstrap EC2 instance 
$ ssh-copy-id <k8s-private-IP>

- next: we should be able to ping to test connectivity to the hosts in the inventory file
$ ansible all -m ping -i inventory









