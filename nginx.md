# windows安装nginx
下载对应包，解压即可 http://nginx.org/en/download.html

# Linux/Centos在线安装nginx（yum/apt方式）
按照教程执行 http://nginx.org/en/linux_packages.html#RHEL-CentOS

# Linux/Centos离线安装nginx（rpm方式）
1. 下载RPM包  
地址：http://nginx.org/packages/centos/7/x86_64/RPMS/
2. 执行以下命令
  ```
  yum install -y nginx-xxx.el7.ngx.x86_64.rpm
  systemctl start nginx.service
  systemctl enable nginx.service
  nginx -v
  systemctl status nginx.service
  ```
3. 查看nginx位置  
`whereis nginx` 默认位置为 /etc/nginx/nginx.conf  
教程说明：https://www.cnblogs.com/xiongzaiqiren/p/12916125.html 

# Linux/Centos离线安装nginx（make编译方式）
1. 下载所需的依赖包
2. make && make install  
教程说明：https://blog.51cto.com/u_15127630/2738677  
https://www.codetd.com/article/11597100  
https://www.cnblogs.com/dandelion200/p/14577480.html  
依赖包搜索网站：http://vault.centos.org/7.4.1708/os/x86_64/Packages/  
Nginx官方文档：https://www.nginx.com/resources/wiki/start/  
从源代码构建nginx教程：https://www.armanism.com/blog/install-nginx-on-ubuntu（推荐）  
http://nginx.org/en/docs/configure.html

# Linux/Centos升级nginx（使用服务信号升级）
1. 查看配置项  
`/usr/local/nginx/sbin/nginx -V`  
2. 重新配置源码和编译  
`./configure   ` (后续参数和之前的配置项一致)  
`make ` (切记 不能使用make install,make 表示编译，而make install 表示的安装过程)  
3. 备份旧的可执行文件  
`cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak`  
4. 查看旧的nginx主进程号  
`cat /usr/local/nginx/logs/nginx.pid` 或 `ps aux |grep nginx |grep -v grep`  
5. kill –USR2 旧版本的nginx主进程号,并替换旧的可执行文件  
```
  kill -USR2 进程号
  cp -rf nginx-1.22.0/objs/nginx /usr/local/nginx/sbin/
  ps aux |grep nginx |grep -v grep
```  
(给nginx发送USR2信号后，nginx会将logs/nginx.pid文件重命名为nginx.pid.oldbin，然后用新的可执行文件启动一个新的nginx主进程和对应的工作进程，并新建一个新的nginx.pid保存新的主进程号)  
```
  cat /usr/local/nginx/logs/nginx.pid   
  cat /usr/local/nginx/logs/nginx.pid.oldbin
```  
6. 给旧的主进程发送WINCH信号，kill -WINCH 旧的主进程号  
`kill -WINCH 进程号`  
7. kill -QUIT旧版本的nginx主进程号    
`kill -QUIT 进程号`  
8. 检查  
```
  ps aux |grep nginx |grep -v grep
  /usr/local/nginx/sbin/nginx -V
```
注：如果想回到旧的nginx不再升级，在执行到 `kill -WINCH`前，给旧的主进程号发送HUP命令，此时nginx不重新读取配置文件的情况下重新启动旧主进程的工作进程  
`kill -HUP 进程号`  
优雅的关闭新的主进程  
`kill -QUIT 进程号`  
教程说明：https://blog.csdn.net/m0_69287945/article/details/125041126

# Linux/Centos升级nginx（使用make命令升级）
1. 1-4和上面一样，然后替换旧的可执行文件  
`cp -rf nginx-1.22.0/objs/nginx /usr/local/nginx/sbin/`  
2. 在新版本的根目录下执行  
`make upgrade`  
教程说明：https://blog.csdn.net/howeres/article/details/124522137
