# In this section we will be deploying our code on docker container
we will be pulling our code from Github build with Maven and deploy on docker container

# we need to setup a docker host
setup an EC2 Instance
# install docker 
sudo yum update

sudo yum install docker

sudo systemctl enable docker.service

sudo systemctl start docker.service

sudo systemctl status docker.service

sudo su - 

# we will be creating a container from a docker image
we are creating a cotainer from a tomcat image from docker hub

first

we pull tomcat image using  docker pull tomcat

we run tomcat container using  docker run -d --name tomcat-container -p 8081:8080 tomcat

and next we make sure the port 8081 is opened on our docker server ruuning on EC2 and access it using the publicIP:8081

- we got 404 page not found, as its a known error with tomcat and we will fix it in the next session

firstly we will have to exec into our container

docker exec -it <conatiner-id> bash

using ls to check through the container we will see that the folder webapps is empty and instead the webapp.dist has the content we need

next: we will copy all d content from the webapp.dist into the webapps dir

cp -R * ../webapps/

and with that done, we can refresh our browser then we can see the tomcat webapps webpage sucessfully

NOTE: whatever we have just done is temporary, if our container gets reloaded we will lose the page once again, we will resolve this in the next step by creating our dockerfile with the folder already copied 

# create a Dockerfile to download tomcat image and copy the webapps.dist to webapps

in this below Docker file we have copied all the content in webapps.dist
```
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/*  /usr/local/tomcat/webapps

```

