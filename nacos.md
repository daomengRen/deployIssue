# windows安装nacos
1. 下载指定版本  
https://github.com/alibaba/nacos/tags
2. 修改bin目录下startup.cmd的MODE  
`set MODE="cluster"`
改成
`set MODE="standalone"`  
3. 本地数据库导入conf目录下的nacos-mysql.sql文件
4. 在conf目录下application.properties添加数据库连接配置
注意：nacos不同版本 连接数据库时，url的参数不一致
```
spring.datasource.platform=mysql
db.num=1
# 1.x版本
db.url.0=jdbc:mysql://127.0.0.1:13310/nacos?useUnicode=true&characterEncoding=utf-8
# 2.0.3版本
# db.url.0=jdbc:mysql://127.0.0.1:13310/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
db.user.0=root
db.password.0=123456
```
5. 双击startup.cmd启动  

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

# Linux安装  
1. 下载.tar.gz  
https://github.com/alibaba/nacos/releases  
2. 数据库导入conf目录下的nacos-mysql.sql文件
3. 执行以下命令
```
mv nacos /usr/local  
cd /usr/local/nacos/conf  
vim application.properties(增加mysql配置)
vim /usr/local/nacos/bin/startup.sh
(修改JAVA_HOME=$HOME/jdk/java 为 JAVA_HOME=/usr/local/java/jdk1.8.0_291)
(修改export MODE="cluster" 为 export MODE="standalone")
sh startup.sh
```
## Linux开机自启
```
vi /etc/systemd/system/nacos.service
# 添加内容如下
[Unit]
Description=nacos
After=network.target

[Service]
Type=forking

# 集群版 见后面 -m standalone 去掉即可
ExecStart=/usr/local/nacos/bin/startup.sh -m standalone
ExecReload=/usr/local/nacos/bin/shutdown.sh
ExecStop=/usr/local/nacos/bin/shutdown.sh
PrivateTmp=true

[Install]
WantedBy=multi-user.target

保存退出后，执行
systemctl daemon-reload #重载系统服务
systemctl enable nacos.service #将服务设置为开机自启
systemctl start nacos.service #启动服务
```
.service 文件格式说明
```
[Unit]
Description:描述，
After：在network.target,auditd.service启动后才启动
ConditionPathExists: 执行条件

[Service]
EnvironmentFile=变量所在文件
ExecStart=执行启动脚本
ExecReload=执行重启命令
ExecStop=执行停止命令
Environment=变量
User=服务运行的用户,
Group=服务运行的用户组
PIDFile=存放PID的文件路径
Restart=fail时重启
PrivateTmp=True表示给服务分配独立的临时空间

[Install]
Alias:服务别名
WangtedBy: 多用户模式下需要的
```
常用命令如下：  
• 启动nacos：systemctl start nacos.service  
• 关闭nacos：systemctl stop nacos.service  
• 设置开机自启：systemctl enable nacos.service  
• 关闭开机自启：systemctl disable nacos.service  
• 查看运行状态：systemctl status nacos.service  

# windows下静默启动nacos
解压版nacos的根目录下新建`start.bat`，内容为：  
```@echo off  
if "%1" == "h" goto begin  
mshta vbscript:createobject("wscript.shell").run("%~nx0 h",0)(window.close)&&exit  
:begin  
E:  
cd E:\Program Files\nacos-server-1.1.4\bin  
startup.cmd -m standalone  
```
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
  ```  
  http://localhost:8848/nacos  
  nacos  
  nacos  
  ```  

3. 找不到JAVA_HOME  
在startup.cmd中设置JAVA_HOME的实际路径  

4. conf目录下的  
schema.sql 是 Derby 数据库的脚本，nacos-mysql.sql 才是 MySQL 的  

5. 官方教程  
https://nacos.io/zh-cn/docs/quick-start.html
