[TOC]

### 导语

一个 LAMP 软件栈通常由多个开源软件组合而成，它们合力驱动一个服务器去运行 web 
站点和应用。LAMP 这个术语实际上是一个由 Linux 操作系统，Apache web 服务器，MySQL 
数据库服务器，以及 PHP 编程环境组合缩略而成的。

在这篇指南中，我们将为一个搭载 CentOS 7 操作系统的服务器安装 LAMP 软件栈。CentOS 
已经满足了 LAMP 软件栈的第一个需求：一个 Linux 操作系统。

预备条件在继续阅读这篇指南之前，请确认你使用具有 root 权限的用户登录了 
CentOS。如果对于当前用户如何取得 root 权限存在疑问，请咨询服务器的管理人员。

这里有一个tip：对于CentOs系统，系统内自带了Apache的安装包，却没有MySQL的安装包。这就意味着Apache可以直接yum install httpd；而MySQL必须首先下载包（详情参下）。


### 安装 Apache

Apache 是目前世界上最广泛使用的 web 服务器，这使得它成为运行网站的绝佳选择。利用 
CentOS 的软件安装包管理系统 yum，我们可以轻易地安装 Apache。它为我们提供了无痛式地从 
CentOS 维护的仓库获取并安装绝大多数软件的方式。你可以前往这里 (https://www.digitalocean.com/community/tutorials/how-to-set-up-and-use-yum-repositories-on-a-centos-6-vps) 获取更多如何使用 yum 
安装包管理系统的指导。

对于我们的目的而言，安装 Apache 只需要在 CentOS 命令终端敲入这条命令就行了：

`$ sudo yum install httpd`

由于使用了 sudo 命令，这些操作将以 root 权限执行。如果当前用户的身份不是 root，CentOS 将会要求你输入当前用户的密码，以验证你的意图。不用一会儿，你的 web 
服务器就安装好了。一旦安装成功，你就可以设置服务器启动时开机启动Apache 服务：

`$ sudo systemctl enable httpd.service`

你可以通过重新启动服务器，然后在命令行终端中敲入这条命令来验证 Apache 服务是否开启了：

`$ sudo systemctl is-enabled httpd.service`

如果你看到了这样的响应：

`enabled`

则说明 Apache 服务已经配置为在服务器启动时自动开启了。

在服务器上手动启动 Apache 服务的命令为：

`$sudo systemctl start httpd.service`

重新启动 Apache：

`$sudo systemctl restart httpd.service`

停止 Apache：

`$sudo systemctl stop httpd.service`

安装目录介绍

```
Apache默认将网站的根目录指向/var/www/html
默认的主配置文件/etc/httpd/conf/httpd.conf
配置存储在的/etc/httpd/conf.d/目录
```

以及如果你的服务器正在运行防火墙，请运行下列命令以允许它进行 HTTP 和 HTTPS 通信：

`$sudo firewall-cmd --permanent --zone=public --add-service=http$sudo firewall-cmd --permanent --zone=public --add-service=https$sudo firewall-cmd --reload`

在 Apache 启动的情况下，你可以在浏览器里访问服务器的公网 IP 地址以验证一切如计划那样顺利地进行.

其他内容可参考： [references](https://blog.51cto.com/13525470/2070375?source=drt)

在我安装Apache时，在html文件夹下没有默认的index.html,可以自己新建一个显示hello world的html文件。

再下一步就是，使用自己的域名替代输入ip地址来建立网页了，首先我们可以到阿里或腾讯等购买一个域名。购买完成后，就在域名控制台设置域名解析服务（实质就是DNS），其中有A，CNAME,和邮件，对于我们个人而言，使用A就足够了（即将你购买的域名同ip地址相关联）。

**这里也有两个大坑： 1） 首先必须要实名认证  2）在域名解析修改后，需要在48以内才会生
效。 因此，一般刚修改过是不会有反应的，需要耐心地等待两天**


### 安装MySQL

请参考：E:\ReviewOfAllLearnedCourse\Summary\Useful_Skills\Vultr(云端）安装Mysql服务器和远程连接

### 安装PHP

以后再看