下载地址：
http://dev.mysql.com/downloads/mysql/5.5.html#downloads
 
centos下解压，配置环境变量即可。

提示安装完成后，输入mysql 看是否安装成功

如果出现如下错误信息：
> ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)
说明mysql服务还没有启动，输入service mysql start启动mysql服务
service mysql start
 
修改密码。

格式：mysqladmin -u用户名 -p旧密码 password 新密码

1. 给root加个密码123456。键入以下命令：
```mysql
  mysqladmin -u root -password 123456
```
2. 再将root的密码改为56789。
```mysql
mysqladmin -u root -p123456 password 56789
```

首次安装时，默认密码为空，可以使用如下命令修改root密码
```mysql
mysqladmin -u root  password mypassword
```

mypassword 为你设定的新密码

然后再次登录

mysql -u root –p
 
rpm包安装的MySQL是不会安装/etc/my.cnf文件的，解决方法，只需要复制/usr/share/mysql目录下的my-huge.cnf 文件到/etc目录，并改名为my.cnf即可
```shell
cp /usr/share/mysql/my-huge.cnf /etc/my.cnf
```
 
配置远程访问
处于安全考虑，Mysql默认是不允许远程访问的，可以使用下面开启远程访问
```mysql
//赋予任何主机访问数据的权限
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'WITH GRANT OPTION;

//使修改生效
FLUSH PRIVILEGES;

```
如果依然不能远程访问的话，那就很可能防火墙的原因了，可以在防火墙中开启3306端口或者干脆关掉防火墙。
```shell
/etc/init.d/iptables status
```  
会得到一系列信息，说明防火墙开着。
```shell
/etc/init.d/iptables stop
```  
永久关闭:
```shell
chkconfig --level 35 iptables off
```  
mysql 重新启动

```shell

service mysqld restart

systemctl restart mysqld
```


Mac下安装：

```shell
brew install mysql@5.7
```

配置环境变量

修改配置文件：/usr/local/etcmy.cnf


Mac下完全卸载Mysql的方法：

```shell
 sudo rm /usr/local/mysql
 sudo rm -rf /usr/local/mysql*
 sudo rm -rf /Library/StartupItems/MySQLCOM
 sudo rm -rf /Library/PreferencePanes/My*
 vim /etc/hostconfig  (and removed the line MYSQLCOM=-YES-)
 rm -rf ~/Library/PreferencePanes/My*
 sudo rm -rf /Library/Receipts/mysql*
 sudo rm -rf /Library/Receipts/mysql*
 sudo rm -rf /var/db/receipts/com.mysql.*

```