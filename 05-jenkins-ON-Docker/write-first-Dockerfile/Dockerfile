# we are installing tomcat on a centos server using Dockerfile
FROM centos:7
# installing java using Run 
RUN  yum install java -y 
# creating a dir inside the /opt folder
RUN mkdir /opt/tomcat
# i would like to go inside the tomcat dir using the WORKDIR
WORKDIR /opt/tomcat
# i would like to download tomcat from google, by coping the tomcat download link address, we could use
#ADD or RUN to download, but if we use RUN then we must use the wget command, but using ADD, we don't need wget
# and we specify . at the end to specify our current working dir, or we could specify /opt/tomcat
ADD https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.73/bin/apache-tomcat-9.0.73.tar.gz .
# we will extract the unzip file using tar -xzvf
RUN tar -xvzf apache-tomcat-9.0.73.tar.gz 
# once it is extracted we are renaming the file, all the file * rename into /opt/tomcat
RUN mv apache-tomcat-9.0.73/* /opt/tomcat
# and by default tomecat run on port 8080, which is the port we have exposed
EXPOSE 8080
# command for starting tomcat, usually we start the tomcat by going inside the "/opt/tomcat/bin/catalina.sh"
# then we run it 
CMD [ "/opt/tomcat/bin/catalina.sh", "run" ]


# next we build our dockerfile using docker build -t <tag-name> .(we use the dot to specify where our Dockerfile is located)