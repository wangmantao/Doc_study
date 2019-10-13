#smb samba 用法

# 灵感
用vsftpd 替代samba
yum install vsftpd 
sudo service vsftpd start

no rout to host -> (host: iptables -F)
permission denied -> (匿名->host: /var/ftp/pub)

1. iptables 开启端口
- iptables-services 安装
- iptables ..... 配置开放端口
- service iptables  save 保存

2. samba 要用的端口
 # iptables -I INPUT -p tcp --dport 139 -j ACCEPT
 # iptables -I INPUT -p tcp --dport 445 -j ACCEPT

3. 列出samba共享文件夹
 smbclient -L ip -U userName
 smbclient //ip/dir -U username%pw

4. 为samba添加用户
	添加的samba用户必须是linux用户(useradd wmt)
	smbpasswd -a wmt
4. 出现denied(拒绝)
	关闭seLinux (setenforce 0)
	永久关闭　　/etc/sysconfig/selinux
5. 在linux本地挂载smb
sudo mount -t cifs //192.168.1.101/share -o /mnt
can't not find in /etc/fstab

# 知识点滴
6. 访问关键：4项和５项
	 
4. setenforce 0 (关闭selinux 对应samba denied)
5. iptables
	iptables -F 清空防火墙规则 (samba无线被路由到)
	iptables -vnL 查看防火墙规则

1. smb://192.168.xxx.xx/
2. netstat -auptn 查看端口建立没
	n : 显示端口号码
3. pdbedit -L
	查看smb当前授权的用户
   pdbedit -Lv
	用户的全部信息
   pdbedit 其它
	-a (添加) -r (修改) -x (删除) -U userName
   改smb用户密码用
	smbpasswd -s -U userName (-s为安静模式)
4. setenforce 0 (关闭selinux 对应samba denied)
5. iptables
	iptables -F 清空防火墙规则 (samba无线被路由到)
	iptables -vnL 查看防火墙规则


# 资源
[阿里云服务器ECS的samba配置方法](https://blog.csdn.net/XHG1993/article/details/78872724) (本地文摘有PDF)
[CentOS Samba 服务器 ](https://www.linuxidc.com/Linux/2016-09/135578.htm)
