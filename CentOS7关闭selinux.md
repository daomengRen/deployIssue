# 临时关闭selinux

`setenforce 0 `

# 查看selinux状态

`getenforce`


# 永久关闭：
文件位置:/etc/selinux/config

`sed -i 's#SELINUX=enforcing#SELINUX=disabled#g' /etc/selinux/config`

`grep 'SELINUX=disabled' /etc/selinux/config`

提示: SELINUX=disabled 的话就是关闭了

关闭之后最好是重启系统,不然可能不生效



# sestatus
查看SELinux的当前状态


Centos 选最小安装
