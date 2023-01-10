# Windows在线安装docker
1. 启用 WSL 2   
```
wsl --install
根据以下链接完成
https://docs.microsoft.com/en-us/windows/wsl/install
https://docs.microsoft.com/windows/wsl/wsl2-kernel
```
2. 安装Docker Desktop 

# Linux/Centos在线安装docker
https://docs.docker.com/engine/install/centos/  
```
yum install -y yum-utils
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
systemctl start docker
docker -v
```

# Linux/Centos离线安装docker（RPM方式）
下载对应版本的.rpm文件  
选择合适的版本https://download.docker.com/linux/centos/  
x86_64/stable/Packages/目录下  
下载`containerd.io` `docker-ce` `docker-ce-cli` `docker-ce-rootless-extras` `docker-ce-selinux` `docker-compose-plugin` `docker-scan-plugin`  
可能需要：`container-selinux` https://pkgs.org/download/container-selinux  
如果出现相互依赖，则执行`rpm -ivh ***.rmp --nodeps --force`强制安装忽略所有依赖关系
   ```
    yum install /path/to/package.rpm
    systemctl start docker
    docker -v
   ```
教程说明https://www.cnblogs.com/aaronthon/p/15772008.html

# Linux/Ubuntu离线安装docker（DEB方式）
`apt-get install lsb-core -y`  
`dpkg  --print-architecture`  
下载对应版本的.deb文件  
选择合适的版本https://download.docker.com/linux/ubuntu/   
dists/目录下选择发行版，进入pool/是工具包，stable/是二进制文件包   
下载`containerd.io` `docker-ce-cli` `docker-ce`   
按顺序安装 `dpkg -i xxxx.deb`  
如果出现相互依赖，则执行`sudo dpkg -i *.deb && apt-get -f install`强制安装忽略所有依赖关系
   ```
    systemctl start docker
    docker -v
   ```
教程说明https://blog.csdn.net/qq_38066812/article/details/127673911

# Linux/Centos离线安装docker（tgz方式）
1. 下载对应版本的tgz  
地址：https://download.docker.com/linux/static/stable/x86_64/
2. 拷贝到服务器上，进行解压   
`tar -zxvf xxx.tgz`  
3. 将解压出来的所有文件，拷贝到`/usr/bin`  
`cp docker/* /usr/bin/`
4. 将docker注册为service，进入/etc/systemd/system/目录,并创建docker.service文件  
`vim /etc/systemd/system/docker.service`  
```
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
ExecReload=/bin/kill -s HUP $MAINPID
# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
#TasksMax=infinity
TimeoutStartSec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
# kill only the docker process, not all processes in the cgroup
KillMode=process
# restart the docker process if it exits prematurely
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target
```
5. 给docker.service文件添加执行权限  
`chmod +x /etc/systemd/system/docker.service`
```
systemctl daemon-reload #重载unit配置文件
systemctl start docker #启动Docker
systemctl enable docker.service #设置开机自启
docker -v
```
教程说明https://blog.csdn.net/li1325169021/article/details/119920931

## 将 Docker 配置为开机启动
`systemctl enable docker.service`  
`systemctl enable containerd.service`

## 安装Docker-Compose
下载地址：https://github.com/docker/compose/releases
```
curl -SL https://github.com/docker/compose/releases/download/v2.6.1/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose -v
```
如果失败，请尝试`ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose`

# 常见问题
1. 官方地址  
https://docs.docker.com/get-docker/
2. 缺少依赖  
![image](https://user-images.githubusercontent.com/46952617/180118423-dabfe687-7092-401f-8b62-f33e3896fc3a.png)  
教程说明：https://www.jianshu.com/p/59ddfea43d28  
3. 下载其他依赖  
Yum-只下载rpm包不安装  
    - 如果只想通过 yum 下载软件的软件包，但是不需要进行安装的话，可以使用 yumdownloader 命令；
yumdownloader 命令在软件包 yum-utils 里面  
        ```
        yum install yum-utils -y

        常用参数说明：
        --destdir 指定下载的软件包存放路径
        --resolve 解决依赖关系并下载所需的包

        yumdownloader --destdir=/tmp --resolve httpd
        #会将所下载的所有rpm放置在tmp目录中
        ```
    - yum命令的参数有很多，其中就有只是下载而不需要安装的命令，并且也会自动解决依赖；
通常和 --downloaddir 参数一起使用。
        ```
        yum install --downloadonly --downloaddir=/tmp/ vsftpd
        #若已经安装了软件，需要使用reinstall进行下载
        #提示没有--downloadonly选项则需要安装yum-plugin-downloadonly软件包；
        yum install yum-plugin-downloadonly
        ```
    - reposync 可以将远端yum仓库里面的包全部下载到本地，构建自己的yum仓库。
        ```
        常用参数说明：
        -r    指定已经本地已经配置的 yum 仓库的 repo源的名称。
        -p    指定下载的路径

        reposync -r epel -p /opt/local_epel
        ```
