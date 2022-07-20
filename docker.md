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

# Linux/Centos离线安装docker
1. 下载对应版本的.rpm文件  
选择合适的版本https://download.docker.com/linux/centos/
x86_64/stable/Packages/目录下
systemctl start docker
```
yum install /path/to/package.rpm
docker -v
```
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
