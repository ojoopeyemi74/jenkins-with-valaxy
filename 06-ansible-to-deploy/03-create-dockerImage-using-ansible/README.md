# we will be creating docker image with the help of ansible
On Ansbile-server (ansadmin)
since we have docker-server IP address already in our inventory file on /etc/ansible/hosts

and we have already been able to ping the docker-host using the ping command from the ansible

# on ansible-server
sudo su - ansadmin
cd /opt/docker
inside docker folder we already have our .war file

 #we are ready to build docker image from the .war file

- next: touch Dockerfile, the Dockerfile will copy our .war file into the webapp on tomcat
```
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/*  /usr/local/tomcat/webapps
```

- next: we create a file for ansible-playbook (playbook-name.yaml) to build a docker image
touch regapp.yaml - the file is insde our docker folder, where we have Dockerfile, and also webapp.war


---

- hosts: 172.31.1.112
  tasks:
  - name: create docker image
    command: docker build -t opeyemiojo/regapp:v1 .
    args:
      chdir: /opt/docker

- next:$ ansible-playbook regapp.yaml 

# we should have our Docker image already on dockerhost

switch to dockerhost which was our target and check docker images

# we can push our new image to dockerhhub for future use using ansible
on the Docker host
- next: make sure you are logged in as root for ansadmin 
sudo su - ansadmin

- next : we login to Dockerhun 
docker login 
opeyemiojo
Opeyemi74

- next: we go to our ansible playbook and to push our image 
```
---

- hosts: 172.31.1.112
  tasks:
  - name: create docker image
    command: docker build -t regapps:latest .
    args:
      chdir: /opt/docker

  - name: create tag to push image onto dockerhub
    command: docker tag regapps:latest opeyemiojo/regapps:latest

  - name: push docker image
    command: docker push opeyemiojo/regapps:latest

```
we can manaully run the ansible-playbook regapp.yaml  to test that its all good

# But we would like our jenkins to automatically push the already build docker image to dockerhub

- next: we go back to our jenkins job configuration and in the command of post build
cd /opt/docker  - # to cd to the docker folder where we have our folder, Dockerfile, *.war , regapp.yaml

```
cd /opt/docker;
ansible-playbook regapp.yml;
sleep 10;
ansible-playbook deploy.regapp.yml

```

- next: we build the job and this time, our docker image should be automatically pushed to Dockerhub

- next: the job is built on ansadmin on ansiblehost 








