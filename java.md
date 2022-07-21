# Linux/Centos安装jdk
1. 下载jdk.tar.gz  
地址：https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
2. 上传服务器并解压  
`tar -zxvf jdk-8u271-linux-x64.tar`
3. 配置环境变量  
```
vim /etc/profile
export JAVA_HOME=/usr/java/jdk1.8.0_271
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib
export PATH=$JAVA_HOME/bin:$PATH
source /etc/profile
java -version
javac -version
```
