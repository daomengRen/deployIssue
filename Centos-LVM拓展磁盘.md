## 拓展 `/` 挂载的 `centos-root` 分区
常见的应用情况  
![image](https://user-images.githubusercontent.com/46952617/211726442-d0a8d81d-c20a-4b6a-9dc4-a82d6b185b1c.png)  
sda的200G空间并未完整使用，所以我们需要把剩余空间拓展到`centos-root`  
`fdisk -l`也能查看硬盘分区大小  
1. 新建分区  
`fdisk /dev/sda    n  p  w`
2. 改变分区类型为LVM  
`fdisk /dev/sda    t  8e w`
3. 同步分区(不重启)  
`partprobe`  
![image](https://user-images.githubusercontent.com/46952617/211728451-ee4372df-1d62-4f59-806c-de0cba46e907.png)  
4. 查看分区格式,看文件系统是否为xfs  
`cat /etc/fstab`
5. 格式化新分区  
`mkfs.xfs /dev/sda3`
6. 查看pv信息  
`pvdisplay`  
7. 创建pv  
`pvcreate /dev/sda3   y`
8. 查看vg信息
`vgdisplay`  
![image](https://user-images.githubusercontent.com/46952617/211729453-79ed9f82-6243-4963-a26c-edbda4d32f23.png)
9. 将新创建的pv假如vg  
`vgextend centos /dev/sda3`
10. 查看lv信息  
`lvdisplay`  
![image](https://user-images.githubusercontent.com/46952617/211729829-5d05e8f5-63c7-4aff-a5e9-8e53c0dbd19b.png)

11. 将vg的剩余空间拓展到lv  
`lvextend -l +100%FREE /dev/centos/root`
12. 拓展XFS文件系统(如果是ext4文件系统)  
`xfs_growfs /dev/centos/root`  
(`resize2fs /dev/centos/root`)
13. 同步分区(不重启)
`partprobe`
14. lsblk成功  
![image](https://user-images.githubusercontent.com/46952617/211730663-b616892e-a41a-4e67-902d-5bd8b722d7a9.png)

**PS:XFS文件系统只支持增大分区空间的情况，不支持减小的情况（切记！！）
硬要减小的话，只能在减小后将逻辑分区重新通过mkfs.xfs命令重新格式化才能挂载上，这样的话这个逻辑分区上原来的数据就丢失了。如果有重要文件，请注意备份**

## 调整已存在的分区大小
比如调整home分区扩大root分区
1. 查看分区  `df -h`
2. 查看目录占用大小  `du -h -x --max-depth=1`
3. 压缩备份`/home`目录 `tar -zcvf /tmp/home.tar.gz /home`
4. 卸载`/home`，如果无法卸载，先终止使用/home文件系统的进程  `fuser -km /home   umount /home`
5. 删除`/home`所在的lv  `lvremove /dev/mapper/centos-home`
6. 扩展`/root`所在的lv，增加800G  `lvextend -L +800G /dev/mapper/centos-root`
7. 扩展`/roo`t文件系统  `xfs_growfs  /dev/mapper/centos-root`
8. 重新创建`home lv`  `lvcreate -L 73G -n /dev/mapper/centos-home`
9. 格式化`home lv`分区  `mkfs.xfs  /dev/mapper/centos-home`
10. 挂载home  `mount /dev/mapper/centos-home`
11. home文件恢复 `tar -zxvf  /tmp/home.tar.gz -C /home/   cd /home/home/   mv * ../`

## Centos下文件系统相关内容
* centos7 默认使用了xfs文件系统
* 不重启的情况下使用`partprobe`重读分区
* xfs文件系统,系统拓展使用`xfs_growfs /dev/root_vg/root`,格式化分区使用`mkfs.xfs /dev/xvdb2`
* ext4文件系统,系统拓展使用`resize2fs /dev/root_vg/root`,格式化分区使用`mkfs -t ext4 /dev/xvdb2`
* 查看分区格式,`cat /etc/fstab`

## 参考资料
https://www.cnblogs.com/zhaojiedi1992/p/zhaojiedi_linux_042_lvm.html  
https://vpssj.net/help/1303.htm  
https://www.cnblogs.com/llxpbbs/articles/11088922.html
