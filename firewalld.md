# centos7防火墙配置

```
systemctl start firewalld
systemctl status firewalld
```

1. 查看防火墙的配置  
```
firewall-cmd --state
firewall-cmd --list-all
```

2. 开放80端口  
```
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --reload    #重新加载防火墙配置才会起作用
```
3. 移除80端口  
```
firewall-cmd --permanent --remove-port=80/tcp
firewall-cmd --reload
```
4. 放通某个端口段  
```
firewall-cmd --permanent --zone=public --add-port=1000-2000/tcp
firewall-cmd --reload
```
5. 放通某个IP访问，默认允许  
```
firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address=192.168.1.169 accept'
firewall-cmd --reload
```
6. 禁止某个IP访问  
```
firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address=10.0.0.42 drop'
firewall-cmd --reload
```
7. 放通某个IP访问某个端口  
```
firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address=192.168.1.169 port protocol=tcp port=6379 accept'
firewall-cmd --reload
```
8. 移除以上规则  
```
firewall-cmd --permanent --remove-rich-rule='rule family="ipv4" source address="192.168.1.169" port port="6379" protocol="tcp" accept'
firewall-cmd --reload
```
9. 放通某个IP段访问  
```
firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address=10.0.0.0/24 accept'
```
