# download maven from the maven website 
https://maven.apache.org/download.cgi

# copy maven binary link and download it using wget Binary tar.gz archive

you can download it on the /opt folder
cd /opt

wget https://dlcdn.apache.org/maven/maven-3/3.9.1/binaries/apache-maven-3.9.1-bin.tar.gz

# next: we extract our download using 

tar -xvzf apache-maven-3.9.1-bin.tar.gz

# we could rename it to maven for easier identification

mv apache-maven-3.9.1-bin.tar.gz maven

# set the variable for maven

cd~
nano .bashrc


# we also need to add java variable to the .bashrc
cd~
find / -name jvm

cd /usr/lib/jvm

find / -name java-11*

we copy /usr/lib/jvm/java-11-openjdk-amd64 and paste it in the .bashrc folder 

```
M2_HOME=/opt/maven
M2=/opt/maven/bin
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2
```
# next we reload the .bashrc using 
source .bashrc

and check using echo $PATH 

# we have to go back to Jenkins UI and install maven plugin
install maven plugin on jenkins

# next we go to global tools configuration
Add JDK

name: Java-11  
Java_home:  /usr/lib/jvm/java-11-openjdk-amd64

Add Maven
name: maven-3
MAVEN_HOME: /opt/maven

Apply and Save

