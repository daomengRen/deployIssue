# windows安装nacos
1. 下载指定版本  
https://github.com/alibaba/nacos/tags
2. 修改bin目录下startup.cmd的MODE  
`set MODE="cluster"`
改成
`set MODE="standalone"`
3. 双击startup.cmd启动  

# docker安装  
1. Clone项目  
`git clone https://github.com/nacos-group/nacos-docker.git`  
`cd nacos-docker`  
2. 单机模式 Derby  
`docker-compose -f example/standalone-derby.yaml up`
3. 单机模式 MySQL 5.7  
`docker-compose -f example/standalone-mysql-5.7.yaml up`   
4. 单机模式 MySQL 8  
`docker-compose -f example/standalone-mysql-8.yaml up`  
5. 集群模式
`docker-compose -f example/cluster-hostname.yaml up`

# windows下静默启动nacos
解压版nacos的根目录下新建`start.bat`，内容为：  
`@echo off
if "%1" == "h" goto begin
mshta vbscript:createobject("wscript.shell").run("%~nx0 h",0)(window.close)&&exit
:begin
E:
cd E:\Program Files\nacos-server-1.1.4\bin
startup.cmd -m standalone`  
注意修改启动目录

# 常见问题
1. 修改配置文件  
对conf目录下的application.properties文件进行修改，server.port为网页端口  
`spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos?useUnicode=true&characterEncoding=utf-8
db.user=root
db.password=123456`  
这是mysql的配置，添加在application.properties末尾

2. 默认的url，账号和密码  
`http://localhost:8848/nacos  nacos nacos`  

3. 找不到JAVA_HOME  
在startup.cmd中设置JAVA_HOME的实际路径  

4. 官方教程  
https://nacos.io/zh-cn/docs/quick-start.html
