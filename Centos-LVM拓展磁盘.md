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
