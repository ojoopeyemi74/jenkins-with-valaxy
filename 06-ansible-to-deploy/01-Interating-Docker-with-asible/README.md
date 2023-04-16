# Integrating Docker with Ansible

# start a new Ec2 instance
- next create ansadmin user and passed
$ useradd ansadmin
$ passwd ansadmin

- next: enable password based login
$ nano /etc/ssh/sshd_config

$ service sshd reload

uncomment the password yes and comment password no

- next: add user to sudoers file
$ visudo

add ansadmin ALL passwd ALl , give ansdmin sudo power


- next: switch to ansadmin and generate ssh keys
$ ssh-keygen

```
- next:  install ansible
$amazon-linux-extras install ansible2 -y
         OR
$ yum install ansible
```

- - - 

# on the Docker host : we will have to create a new user named ansadmin

we will be using this new user as our user admin

- next: Add ansadmin too sudoers files
- next: Enable password based login 
$ nano /etc/ssh/sshd_config


# and on the Ansible node
- next: Add to docker ip inside the default inventory file
$ nano /etc/ansible/hosts

you can delete all the content in the hosts file and add the private IP of your dockerserver

- next: we will have to switch back to our ansible server on the ansadmin user where we generated our SSH key earlier
- next: we will copy the pub key to our docker target
we could manually copy the ssh pub key or use the below command
$ ssh-copy-id <target-ip>

- next: we will validate connection to the docker target from ansible host in the ansadmin user
$ ansible all -m ping 
 