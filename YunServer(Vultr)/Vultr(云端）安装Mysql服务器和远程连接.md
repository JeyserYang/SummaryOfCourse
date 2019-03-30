[TOC]

整个步骤在MySQL官网上也有，这说明了官网的重要性：[mysql-yum-repo](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/)

#### 连接远程服务器

C:\Users\JeryserYang>ssh root@45.76.198.83

#### 下载和安装rpm包

wget https://dev.mysql.com/get/mysql80-community-release-el7-2.noarch（下载rpm包）

rpm -Uvh mysql57-community-release-el7-11.noarch.rpm

#### 安装MySQL

yum install mysql-community-server（安装mysql）

#### 启动MySQL服务器

service mysqld start

#### 查看状态

service mysqld status

#### 查看MySQL最初密码用于登陆

grep 'temporary password' /var/log/mysqld.log（查看mysql最初的root密码用于登录）

#### 登陆

mysql -uroot -p（登录mysql）

#### 修改密码

ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';

**注意：这里出现了Your password does not satisfy the current policy requirements的错误，这是因为高版本的MySQL增加了密码强度插件validate_password，因此如果想要使用简单密码，必须修改这里的内容**

**请参考[密码强度修改](https://blog.csdn.net/kuluzs/article/details/51924374)**

至此，MySQL服务器就安装在云端了，接下来是使用本地客户端连接远程服务器。

#### 远程连接

1. 在cmd下客户端所在的文件下输入连接（3306是mysql运行时的默认端口号）

`C:\Program Files\MySQL\MySQL Server 8.0\bin>mysql -h45.76.198.83 -P3306 -uroot -p`

**注：这里出现了两大错误（1130和2003）**

**在我这里2003的原因是3306端口未开放，解决如下：**

> 使用nestat命令查看3306端口状态

        [root~vultr]# netstat -an | grep 3306 
        tcp 0 0 (服务器本地ip地址):3306 0.0.0.0:* LISTEN

> 设置3306端口可以访问

        /sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT

> * 在安装MySQL服务器的主机上（我这里是vultr的云服务器）运行如下命令

        //进入MySQL服务器

        >mysql -h localhost -u root

        //赋予任何主机访问数据的权限

        mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;

        //使修改生效

        mysql>FLUSH PRIVILEGES

        //退出MySQL服务器

        mysql>EXIT

> * 1103问题解决：修改表使得mysql中user表的root用户可以从任意主机访问

        mysql -u root –p

        mysql>use mysql;

        mysql>update user set host = '%' where user = 'root';

        mysql>flush privileges; 

        mysql>select host, user from user;

差不多这样应该就能解决了，如果实在不行，再到网上找。
