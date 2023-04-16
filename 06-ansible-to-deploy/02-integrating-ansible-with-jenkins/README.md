# integrating Ansible with Jenkins
we will be intrgrating anisble to Jenkins so that when Jenkins do the building activities, also Ansbile will deploy 

# on ansible-server 
```
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

```

# we go to Jenkins UI

- next: we go to Manage jenkins
- necxt: Configure system
- next: since we have Publish over SSH plugins already, then we Add a new server: ansadmin
and test to be sure its a sucess

- next: we start a new Job
and we will be having our post build to publish over SSH and server will be ansadmin server we just added in the Configure system

Remote directory
//opt//docker

- next: we make sure on our ansible-server (ansadmin ) we create a folder inside /opt/docker, where our post build *.war file will go to 
on ansible-server 
sudo su - ansadmin
cd /opt
sudo mkdir docker
sudo chown ansadmin:ansadmin docker  # we need the docker folder to be owned by ansadmin as that is our server on Jenkins

- next: after building the Job, now our .war file should be inside /opt/docker 
