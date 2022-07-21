# Linux/Centos离线安装libreoffice
1. 下载安装包  
下载地址：https://www.libreoffice.org/download/download/  
旧版本下载地址：https://downloadarchive.documentfoundation.org/libreoffice/old/7.1.0.2/rpm/x86_64/
2. 上传服务器，解压文件  
`tar -zxvf LibreOffice_7.1.0.2_Linux_x86-64_rpm.tar.gz`
3. 进入文件RPMS目录下  
`cd /opt/libreoffice7.1/LibreOffice_7.1.0.2_Linux_x86-64_rpm/RPMS`
4. 安装rpm文件  
`rpm -Uivh *.rpm --nodeps`

5. 测试是否安装成功(非必要)  
```
/usr/bin/libreoffice7.1 --headless --accept="socket,host=0.0.0.0,port=8100;urp;" --nofirststartwizard
#速度有点慢

#可能出现错误 /opt/libreoffice7.1/program/oosplash: error while loading shared libraries: libXinerama.so.1: cannot open shared object file: No such file or directory
原因：缺少相关依赖，需全部安装才可启动成功
avahi-libs-0.6.31-20.el7.x86_64.rpm
cairo-1.15.12-4.el7.x86_64.rpm
cups-libs-1.6.3-51.el7.x86_64.rpm
fontconfig-2.13.0-4.3.el7.x86_64.rpm
libglvnd-1.0.1-0.8.git5baa1e5.el7.x86_64.rpm
libglvnd-egl-1.0.1-0.8.git5baa1e5.el7.x86_64.rpm
libglvnd-glx-1.0.1-0.8.git5baa1e5.el7.x86_64.rpm
libICE-1.0.9-9.el7.x86_64.rpm
libSM-1.2.2-2.el7.x86_64.rpm
libX11-1.6.7-2.el7.x86_64.rpm
libXau-1.0.8-2.1.el7.x86_64.rpm
libxcb-1.13-1.el7.x86_64.rpm
libXext-1.3.3-3.el7.x86_64.rpm
libXinerama-1.1.3-2.1.el7.x86_64.rpm
libXrender-0.9.10-1.el7.x86_64.rpm
libpng15-1.5.30-7.el8.x86_64.rpm

链接：https://pan.baidu.com/s/1kodG6JwHB9_cSo1nRsoWKw
提取码：6666
```
教程说明：https://blog.csdn.net/Y_hanxiong/article/details/124392435
