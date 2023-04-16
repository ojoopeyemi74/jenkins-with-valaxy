# first we have to install java 11 on our vm (EC2)
sudo apt update

sudo apt install default-jre

sudo apt install default-jdk -y 

java -version

# install tomcat server
sudo to root  sudo su -

google tomcat download -  https://tomcat.apache.org/download-90.cgi

copy the tomcat link address - https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.73/bin/apache-tomcat-9.0.73.tar.gz

wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.73/bin/apache-tomcat-9.0.73.tar.gz 

next: extract the file using - tar -xvzf apache-tomcat-9.0.73.tar.gz

mv apache-tomcat-9.0.73 tomcat

cd tomcat

cd bin/

ls

$ ./startup.sh       to start tomcat on port 8080 already opened

