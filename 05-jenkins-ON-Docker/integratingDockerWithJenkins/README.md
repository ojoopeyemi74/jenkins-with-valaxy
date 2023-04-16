# first to integrate docker with jenkins its best practise to create a dedicated docker admin user
and make sure this user is added to the docker group

- first we check our current users using 
cat /etc/passwd

- we check the groups
cat /etc/group

- we will create a user named dockeradmin and setup a password
useradd dockeradmin

passwd dockeradmin

- we should add the dockeradmin user to the docker group
if you check the dockeradmin user using  id dockeradmin you can see its only with dockeradmin user and group

- add dockeradmin user to docker group
usermod -aG docker dockeradmin

cat /etc/group and you can see dockeradmin as been added to docker group



# we will copy artifact from jenkins to docker host using "publish Over SSH plugins" using password
switch to the root   sudo su -
we have to modidy the sshd config file to allow password on EC2

nano /etc/ssh/sshd_config

we look for the file with Password as no and we remove the comment to yes
```
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication yes
#PermitEmptyPasswords no
#PasswordAuthentication no
```
- next we reload the ssh server, dont't stop and start, so we dont lose connection to ssh

service sshd reload

we should be able to ssh into our dockeradmin user
cd to the folder where you have your ssh on your computer

ssh dockeradmin@<ipaddress-of-d-ec2>

# we have to add docker host to jenkins " configure system" so we can communicate with dockerhost from jenkins
- we go to manage plugins and install plugins over SSH

- we go to configure system

Name : docker-server

Hostname : public or private ip address of our docker server on ec2

Username : dockeradmin

Passphrase / Password : Opeyemi74   -- the password we have set for our dockeradmin user

- then test configuration it should be sucess

# HOW TO INTEGRATE DOCKER HOST WITH JENKINS USING SSH KEYS

# We want to send build over SSH to dockerhost using Add post-build action(send build artifact over SSh)

- next: Source files - /webapp/target/.*war its best to use **/*.war  ( that is the path to our war file in the workspace)

-next: remove prefix- /webapp/target

-next: Remote directory- where we want the war file to be store on our docker server, it could be in the /opt/docker

# we should have our war file now on dockeradmin host 
sudo su -dockeradmin

ls

there should be the webapp.war

# we want to make a docker image out of our war file now( our artifact)
next: we should copy the webapp.war file from dockeradmin user to dockerhost server and create a dir named docker inside /opt

so that in the future we can also build our image and run our containers from the dockerhost server for easier refrence

- next: we switch to dockerhost root : sudo su -

cd /opt
mkdir docker

- next: because we have told jenkins that dockeradmin is our user then we need to chown of /docker to dockeradmin:dockeradmin

- next: we go back to our jenkins job configuration
change the Remote directory : //opt//docker
apply and save and build job

- then the artifact would be on the dockerhost /opt/docker

- next:chown dockeradmin:dockeradmin docker

- next: we mv Dockerfile from into the /opt/docker
( and also chown of Dockerfile: chown dockeradmin:dockeradmin Dockerfile)

- next : now in the /opt/docker we have Dockerfile and also webapp.war

- next: we already have our Dockerfile 
```
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/*  /usr/local/tomcat/webapps
```

- next: we need to build a new Docker image by updating our Dockerfile with a copy of our new build artifact

we use the COPY to copy the artifact into the /usr/local/tomcat/webapps
```
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/*  /usr/local/tomcat/webapps
COPY ./*.war /usr/local/tomcat/webapps
```

- next: build -t new-job .

- next: docker run -d --name <container-name> -p 8081:8080 new-job

- next: we can access our app using <public-ip:8081/webapp>

# We want to Automate build and deployment on Docker container
- next: on the jenkins job configuration we will add all the commands 
Exec command:

```
cd /opt/docker;
docker build -t regappv1 .;
docker run -d --name regapp -p 8084:8080 regappv1
```

# we would like our job to automatically run when we update the github
and in this case if we update the github and the job starts running it will fail because we already
had a container with the same name regappv1

so to Solve this issue

- next: we update the Exec command

with this command docker will firstly delete any container with that name and run a new container with the same name
```
cd /opt/docker;
docker build -t regappv1 .;
docker stop regapp;
docker rm regapp;
docker run -d --name regapp -p 8084:8080 regappv1
``

