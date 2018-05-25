**Installing Jenkins using war on top of TOMCAT with JAVA jdk**
  
links: 
-----
Jenkins     : http://mirrors.jenkins.io/war-stable/latest/jenkins.war
Oracle JDK  : http://www.oracle.com/technetwork/java/javase/downloads
Tomcat      : http://tomcat.apache.org/download-70.cgi
Tomcat8     : http://redrockdigimark.com/apachemirror/tomcat/tomcat-8/v8.5.15/bin/apache-tomcat-8.5.15.tar.gz


Steps:

1. Installed a Ubunttu 14.04 on Virtual Machine ( IP : 192.168.2.92 )
2. Installed Oracale JDK 1.8
3. Installed Tomcat 8 
4. Installed Jenkins. ( username: jenkins, password: jenkins)




Oracle JDK/JAVA installation.
------------------------
Step 1: Below is the url to download oracle 1.8

wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.tar.gz
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" <Copy the link url here from official website: http://www.oracle.com/technetwork/java/javase/downloads>

Move the tar file into specific directory.

mkdir /opt/jdk
tar -xvzf jdk-8u131-linux-x64.tar.gz -C /opt/jdk/

root@ubuntu:~# update-alternatives --install /usr/bin/java java /opt/jdk/jdk1.8.0_131/bin/java 300
root@ubuntu:~# update-alternatives --install /usr/bin/javac javac /opt/jdk/jdk1.8.0_131/bin/javac 300


root@ubuntu:~# java -version
java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)

root@ubuntu:~# update-alternatives --display java
java - auto mode
  link currently points to /opt/jdk/jdk1.8.0_131/bin/java
/opt/jdk/jdk1.8.0_131/bin/java - priority 300
Current 'best' version is '/opt/jdk/jdk1.8.0_131/bin/java'.
root@ubuntu:~# update-alternatives --display javac
javac - auto mode
  link currently points to /opt/jdk/jdk1.8.0_131/bin/javac
/opt/jdk/jdk1.8.0_131/bin/javac - priority 300
Current 'best' version is '/opt/jdk/jdk1.8.0_131/bin/javac'.
root@ubuntu:~# 

Set Environment Variable: for JAVA_HOME

root@ubuntu:~# cat >> /etc/bash.bashrc <<EOF
> export JAVA_HOME=/opt/jdk/jdk1.8.0_131
> EOF

root@ubuntu:~# source /etc/bash.bashrc

root@ubuntu:~# env | grep JAVA_HOME
JAVA_HOME=/opt/jdk/jdk1.8.0_131
root@ubuntu:~# 

----------------------------------------------------------------------------------------------------------------
Tomcat Installation:
---------------------

Check for the Tomcat version for your need : http://tomcat.apache.org/download-70.cgi

root@ubuntu:~# mkdir /opt/tomcat8
root@ubuntu:~# wget http://redrockdigimark.com/apachemirror/tomcat/tomcat-8/v8.5.15/bin/apache-tomcat-8.5.15.tar.gz
root@ubuntu:~# tar -xvzf apache-tomcat-8.5.15.tar.gz -C /opt/tomcat8/

root@ubuntu:~# groupadd tomcat
root@ubuntu:~# useradd -s /bin/false -g tomcat -d /opt/tomcat8 tomcat
root@ubuntu:~# chown -R tomcat:tomcat /opt/tomcat8/

root@ubuntu:/opt/tomcat8/apache-tomcat-8.5.15# find / -name *conf
/opt/tomcat8/apache-tomcat-8.5.15/conf

root@ubuntu:~# cat >> /etc/bash.bashrc<< EOF
> export CATALINA_HOME=/opt/tomcat8/apache-tomcat-8.5.15
> export CATALINA_BASE=/opt/tomcat8/apache-tomcat-8.5.15
> EOF

root@ubuntu:~# source /etc/bash.bashrc

root@ubuntu:~# env | grep CATA
CATALINA_HOME=/opt/tomcat8/apache-tomcat-8.5.15
CATALINA_BASE=/opt/tomcat8/apache-tomcat-8.5.15

root@ubuntu:~# su tomcat -s /bin/bash
tomcat@ubuntu:/root$ cd
tomcat@ubuntu:~$ pwd
/opt/tomcat8
tomcat@ubuntu:~$ ls -l /opt/tomcat8/
total 4
drwxr-xr-x 9 tomcat tomcat 4096 May 30 11:23 apache-tomcat-8.5.15
tomcat@ubuntu:~$ 

Starting TOMCAT
--------------
tomcat@ubuntu:~$ $CATALINA_HOME/bin/startup.sh
Using CATALINA_BASE:   /opt/tomcat8/apache-tomcat-8.5.15
Using CATALINA_HOME:   /opt/tomcat8/apache-tomcat-8.5.15
Using CATALINA_TMPDIR: /opt/tomcat8/apache-tomcat-8.5.15/temp
Using JRE_HOME:        /opt/jdk/jdk1.8.0_131
Using CLASSPATH:       /opt/tomcat8/apache-tomcat-8.5.15/bin/bootstrap.jar:/opt/tomcat8/apache-tomcat-8.5.15/bin/tomcat-juli.jar
Tomcat started.
tomcat@ubuntu:~$ 

tomcat@ubuntu:~$ netstat -tulpn | grep 8080
tcp6       0      0 :::8080                 :::*                    LISTEN      1642/java       
tomcat@ubuntu:~$ ps -ef
root      1610  1120  0 11:36 pts/0    00:00:00 su tomcat -s /bin/bash
tomcat    1611  1610  0 11:36 pts/0    00:00:00 bash
tomcat    1642     1  3 11:47 pts/0    00:00:02 /opt/jdk/jdk1.8.0_131/bin/java -Djava.util.logging.config.file=/opt/tomcat8/apache-tomcat-8.5.15/
tomcat    1686  1611  0 11:48 pts/0    00:00:00 ps -ef

To Stop or Shutdown TOMCAT : tomcat@ubuntu:~$ $CATALINA_HOME/bin/shutdown.sh

To Access Tomcat UI Openinweb browser with 8080 port

http://192.168.2.92:8080/

------------------------------------------------------------------------------------------------------
Installing Jenkins
-------------------

. Download Jenkins war and move it to Tomcat WebApps Directory.
. And then open in browser with jenikins

wget wget wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war
mv jenkins.war /opt/tomcat8/apache-tomcat-8.5.15/webapps/

Then Open in Browser

http://192.168.2.92:8080/jenkins/

. After Getting dashboard it will ask for password. So go to the location and copy and paste the password from specified location.

like:  cat /opt/tomcat8/.jenkins/secrets/initialAdminPassword
       d68fbac621224ac381cffb87e4735fbe

Then Install the required plugins and create the username and password for jenkins user.

Now you do your deployements as you like.

username: jenkins
password: jenkins
-----------------------------------------------------------------------------------------------------------

Reference Links:

https://oracle-base.com/articles/linux/apache-tomcat-7-installation-on-linux
https://www.digitalocean.com/community/tutorials/how-to-manually-install-oracle-java-on-a-debian-or-ubuntu-vps


