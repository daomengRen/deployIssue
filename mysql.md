# 解压缩版mysql迁移
1. 复制整个目录（包含data）
2. 安装mysql服务  `mysqld --install mysql` (mysql为自定义服务名）   
卸载mysql服务  `mysqld --remove mysql`
3. 启动mysql服务  `net start mysql`  
关闭服务  `net stop mysql`

# 解压缩版mysql完整版安装
1. 下载mysql数据库压缩包 下载地址：https://dev.mysql.com/downloads/mysql/
2. 解压缩，放到自定义目录
3. 在解压后的根目录，创建my.ini文件（文件中包含mysql的端口，安装路径和数据存放路径，自行修改）  
字段为port,basedir,datadir
4. 在bin目录下执行`mysqld --initialize --console`  
初始化mysql，会生成data目录，并获得初始密码  
data目录里面err后缀的文件保存了数据库密码
 A temporary password is generated for root@localhost: 7WtycAe-X8fq
5. 安装mysql服务  `mysqld --install mysql` (mysql为自定义服务名）   
卸载mysql服务  `mysqld --remove mysql`
6. 启动mysql服务  `net start mysql`  
关闭服务  `net stop mysql`
   
# 常见问题
1. 连接数据库  
mysql -uroot -p
2. 修改root密码，（ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)）  
`use mysql;`  
`update user set authentication_string=password('新密码') where user='root';`  
`flush privileges;`
3. 跳过mysql登录密码校验  
在my.ini的[mysqld]在添加`skip-grant-tables`或`skip-grant-tables = true`，然后关闭mysql服务，开启mysql服务
4. 官方教程  
https://dev.mysql.com/doc/refman/5.7/en/installing.html
