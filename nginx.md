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
