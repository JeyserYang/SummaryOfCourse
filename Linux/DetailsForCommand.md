[TOC]

#### man(Most Significant)

man命令是linux下的帮助指令，通过man可以查看Linux中的指令帮助、配置文件帮助和编程帮助等信息。

* 语法(Usage)

`Usage: man [OPTION...] [SECTION] PAGE...`

* 选项(Option)

```
-a：在所有的man帮助手册中搜索；
-f：等价于whatis指令，显示给定关键字的简短描述信息；
-P：指定内容时使用分页程序；
-M：指定man手册的搜索的路径。
```

* 参数(Section)

man 命令是按照章节存储的，Linux的man手册共有以下几个章节:

```
1.用户命令章节，所有用户都可以使用的

2.系统调用命令章节

3.c库调用

4.设备及特殊文件

5.配置文件格式及相关参数

6.游戏

7.杂项

8.管理命令
```

我们输入“man ls”,在屏幕的左上角会显示“”，在这里“LS”表示手册名称，而“（1）”表示该手册位于第一章节。

man是按照手册的章节号的顺序进行搜索的，比如： man sleep只会显示sleep在章节1中的信息，相当于命令“man 1 sleep”。如果想查看
库函数sleep，就要输入： man 3 sleep

* man配置文件

```
Centos 6：/etc/man.conf

Centos 7：/etc/man_db.conf
```

* man手册段落含义

```
NAME：命令的名称及简要说明
DESCRIPTION:命令功能的详细描述
OPTIONS：所支持的选项的相关说明
SYNOPSIS：使用格式
EXAMPLES：使用示例
NOTES：相关注意事项
FILES：相关的配置文件
SEE ALSO：相关参考
```

* 通过man命令获得命令的帮助信息页当中有一些用符号标记的内容(也适用于其他命令，如ls等)，这些符号的意义是：

```
[]：可选内容

<>：必选内容

|:二选一

...：同类内容可以有多个
```

* man手册可以进行的操作

|   按键  |         实现功能         |
|---------|--------------------------|
| 空格键  | 向下翻一页               |
| Pg Dn   | 向下翻一页               |
| Pg Up   | 向上翻一页               |
| Home    | 去到第一页               |
| End     | 去到最后一页             |
| /string | 向下搜索string这个字符串 |
| ?string | 向上搜索string这个字符串 |
| q       | 退出                     |


* 出现错误

`No manual entry for {command}`

To fix this, first check that you have the following rpms installed:

```
$ yum install -y man-pages
$ yum install -y man-db
```

Next run the mandb command to populate/update the mandb database:

`$ mandb`


#### ls

Linux ls命令用于显示指定工作目录下之内容（列出目前工作目录所含之文件及子目录)。

```
注：所有的命令都可以组合，有两种写法： 1） ls -l -i  2) ls -li

显示颜色
[root@vultr ~]# ls --color=auto
bbr.sh  bbr.sh.1  install_bbr.log  mysql80-community-release-el7-2.noarch.rpm  shadowsocksR.log  shadowsocksR.sh

显示权限和日期大小
[root@vultr ~]# ll
total 164
-rwxr-xr-x 1 root root 13173 Mar 11 09:00 bbr.sh
-rw-r--r-- 1 root root 13173 Mar 12 18:23 bbr.sh.1
-rw-r--r-- 1 root root    71 Mar 12 18:23 install_bbr.log
-rw-r--r-- 1 root root 25892 Jan 18 06:02 mysql80-community-release-el7-2.noarch.rpm
-rw-r--r-- 1 root root 78892 Mar 11 08:59 shadowsocksR.log
-rwxr-xr-x 1 root root 16212 Mar 11 08:56 shadowsocksR.sh

显示所有文件（包括隐藏文件）
[root@vultr ~]# ls -a
.              .bash_logout   bbr.sh    install_bbr.log                             .mysql_history    shadowsocksR.sh
..             .bash_profile  bbr.sh.1  .lesshst                                    .pki              .tcshrc
.bash_history  .bashrc        .cshrc    mysql80-community-release-el7-2.noarch.rpm  shadowsocksR.log

显示inode结点号
[root@vultr ~]# ls -i
22783 bbr.sh    22784 install_bbr.log                             23195 shadowsocksR.log
 9110 bbr.sh.1  26606 mysql80-community-release-el7-2.noarch.rpm  23189 shadowsocksR.sh

列出目前工作目录下所有名称是 s 开头的文件，越新的排越后面 :
[root@vultr ~]# ls -ltr s*
-rwxr-xr-x 1 root root 16212 Mar 11 08:56 shadowsocksR.sh
-rw-r--r-- 1 root root 78892 Mar 11 08:59 shadowsocksR.log
```

颜色含义如下：

        1. 蓝色-->目录

        2. 绿色-->可执行文件

        3. 红色-->压缩文件

        4. 浅蓝色-->链接文件

        5. 灰色-->其他文件

#### alias(别名)

可以给常用的命令起别名，使其输入更加简单

```
直接运行alias可以看到所有的别名的设置
[root@vultr ~]# alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
```

* 临时设置别名，只在当前shell中有用

```
设置显示inode号的别名，就和上面显示的一样
alias li = 'ls -i --color=auto'
```

* 永久设置别名

```
更改~/.bashrc或/etc/bashrc，两者的区别，前者是针对当前用户，后者针对全局用户
[root@vultr ~]# vi .bashrc
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
alias li='ls -i'        //添加这一行

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

没有重启之前查看alias是没有生效的
[root@vultr ~]# li
bash: li: command not found

重启过后查看，已经生效
[root@vultr ~]# li
22783 bbr.sh    22784 install_bbr.log                             23195 shadowsocksR.log
 9110 bbr.sh.1  26606 mysql80-community-release-el7-2.noarch.rpm  23189 shadowsocksR.sh
```

#### ln（硬链接和软件接）

【硬连接】
硬连接指通过索引节点来进行连接。在Linux的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号(Inode Index)。**在Linux中，多个文件名指向同一索引节点是存在的**。一般这种连接就是硬连接。硬连接的作用是允许一个文件拥有多个有效路径名，这样用户就可以建立硬连接到重要文件，以防止“误删”的功能。其原因如上所述，**因为对应该目录的索引节点有一个以上的连接。只删除一个连接并不影响索引节点本身和其它的连接，只有当最后一个连接被删除后，文件的数据块及目录的连接才会被释放**。也就是说，文件真正删除的条件是与之相关的所有硬连接文件均被删除。

【软连接】
另外一种连接称之为符号连接（Symbolic Link），也叫软连接。**软链接文件有类似于Windows的快捷方式。它实际上是一个特殊的文件。在符号连接中，文件实际上是一个文本文件，其中包含的有另一文件的位置信息。**

```
测试
[root@vultr ~]# mkdir test
[root@vultr ~]# cd test
[root@vultr test]# ls
[root@vultr test]# vi test.log
[root@vultr test]# ln test.log test1.log
[root@vultr test]# ln -s test.long test2.log
[root@vultr test]# li
516133 test1.log  516127 test2.log  516133 test.log

从最终结果中可以看到，硬链接的inode是一样的，而软链接不同。（inode结点是和文件一一对应的）
```

#### file（查看文件类型）

```
[root@vultr ~]# vi temp.html
[root@vultr ~]# file temp.html
temp.html: HTML document, ASCII text
[root@vultr ~]# file -i temp.html
temp.html: text/html; charset=us-ascii
```

#### more

more命令，功能类似 cat ，cat命令是整个文件的内容从上到下显示在屏幕上。 more会以一页一页的显示方便使用者逐页阅读，而最基本
的指令就是按空白键（space）就往下一页显示，按 b 键就会往回（back）一页显示，而且还有搜寻字串的功能。more命令从前向后读取文件
，因此在启动时就加载整个文件。

```
常用操作命令：
Enter    向下n行，需要定义。默认为1行
空格键  向下滚动一屏
Ctrl+B  返回上一屏
=       输出当前行的行号
：f     输出文件名和当前行的行号
V      调用vi编辑器
!命令   调用Shell，并执行命令 
q       退出more

列一个目录下的文件，由于内容太多，我们应该学会用more来分页显示。这得和管道 | 结合起来 
ls -l  | more -5
```

#### 环境变量

Linux是一个多用户多任务的操作系统，可以在Linux中为不同的用户设置不同的运行环境，具体做法是设置不同用户的环境变量。

* Linux环境变量分类
```
一、按照生命周期来分，Linux环境变量可以分为两类：
1、永久的：需要用户修改相关的配置文件，变量永久生效。
2、临时的：用户利用export命令，在当前终端下声明环境变量，关闭Shell终端失效。

二、按照作用域来分，Linux环境变量可以分为：
1、系统环境变量：系统环境变量对该系统中所有用户都有效。
2、用户环境变量：顾名思义，这种类型的环境变量只对特定的用户有效。
```
* Linux设置环境变量的方法
```
一、在/etc/profile文件中添加变量 对所有用户生效（永久的）
用vim在文件/etc/profile文件中增加变量，该变量将会对Linux下所有用户有效，并且是“永久的”。
例如：编辑/etc/profile文件，添加CLASSPATH变量
vim /etc/profile    
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
注：修改文件后要想马上生效还要运行source /etc/profile不然只能在下次重进此用户时生效。

二、在用户目录下的.bash_profile文件中增加变量 【对单一用户生效（永久的）】
用vim ~/.bash_profile文件中增加变量，改变量仅会对当前用户有效，并且是“永久的”。
vim ~/.bash.profile
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib

三、直接运行export命令定义变量 【只对当前shell（BASH）有效（临时的）】
在shell的命令行下直接使用export 变量名=变量值
定义变量，该变量只在当前的shell（BASH）或其子shell（BASH）下是有效的，shell关闭了，变量也就失效了，再打开新shell时就没有这个变量，需要使用的话还需要重新定义。
```
* Linux环境变量使用
```
一、Linux中常见的环境变量有：

1.PATH：可执行程序的搜索路径，当要求系统运行一个程序，而没告诉系统它的具体路径时，系统就要在PTAH值的路径中寻找此程序，找到去执行
PATH声明用法：

PATH=$PAHT:<PATH 1>:<PATH 2>:<PATH 3>:--------:< PATH  n >
export PATH

你可以自己加上指定的路径，中间用冒号隔开。环境变量更改后，在用户下次登陆时生效。可以利用echo $PATH查看当前当前系统PATH路径。

注： 1）这里每个路径使用冒号分隔  2） 如果我们在linux系统上安装了一个可执行软件，就可以将软件路径放入PATH环境变量后面，这样就可
以直接像使用 ls 命令一样打开软件运行了。 

HOME：指定用户的主工作目录（即用户登陆到Linux系统中时，默认的目录）。
HISTSIZE：指保存历史命令记录的条数。
LOGNAME：指当前用户的登录名。
HOSTNAME：指主机的名称，许多应用程序如果要用到主机名的话，通常是从这个环境变量中来取得的
SHELL：指当前用户用的是哪种Shell。
LANG/LANGUGE：和语言相关的环境变量，使用多种语言的用户可以修改此环境变量。
MAIL：指当前用户的邮件存放目录。

注意：上述变量的名字并不固定，如HOSTNAME在某些Linux系统中可能设置成HOST
```
* 查看和修改环境变量
```
echo         显示某个环境变量值 echo $PATH
export   设置一个新的环境变量 export HELLO="hello" (可以无引号)
env      显示所有环境变量
set      显示本地定义的shell变量
unset        清除环境变量 unset HELLO
readonly     设置只读环境变量 readonly HELLO

C程序调用环境变量函数:
getenv()返回一个环境变量。
setenv()设置一个环境变量。
unsetenv()清除一个环境变量
```

#### 其他（Whereis，whatis，su，df，du，yum，rpm，wget等）

```
[root@jeyser ~]# whereis ls                         //查看命令路径
ls: /usr/bin/ls /usr/share/man/man1p/ls.1p.gz
[root@jeyser ~]# whatis ls                          //查看命令简要功能
ls (1p)              - list directory contents

su命令用来切换用户

[root@jeyser ~]# df -h                              //磁盘使用情况
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        462M     0  462M   0% /dev
tmpfs           493M     0  493M   0% /dev/shm
tmpfs           493M   19M  474M   4% /run
tmpfs           493M     0  493M   0% /sys/fs/cgroup
/dev/vda1        25G  3.7G   20G  16% /
tmpfs            99M     0   99M   0% /run/user/0

[root@jeyser ~]# du -h                              //当前目录和子目录大小
4.0K    ./.pki/nssdb
8.0K    ./.pki
208K    .
```

* yum（安装软件包）
```
I. 介绍

yum（ Yellow dog Updater, Modified）是一个在Fedora和RedHat以及SUSE中的Shell前端软件包管理器。
基於RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次
下载、安装。
yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

II. yum 语法

yum [options] [command] [package ...]

options：可选，选项包括-h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。
command：要进行的操作。
package操作的对象。

III. yum常用命令

1.列出所有可更新的软件清单命令：yum check-update
2.更新所有软件命令：yum update
3.仅安装指定的软件命令：yum install <package_name>
4.仅更新指定的软件命令：yum update <package_name>
5.列出所有可安裝的软件清单命令：yum list
6.删除软件包命令：yum remove <package_name>
7.查找软件包 命令：yum search <keyword>
8.清除缓存命令:
yum clean packages: 清除缓存目录下的软件包
yum clean headers: 清除缓存目录下的 headers
yum clean oldheaders: 清除缓存目录下旧的 headers
yum clean, yum clean all (= yum clean packages; yum clean oldheaders) :清除缓存目录下的软件包及旧的headers

IV. 例如
yum install httpd               //安装apache
```

* rpm（安装软件包）
```
RPM是RedHat Package Manager（RedHat软件包管理工具）类似Windows里面的“添加/删除程序”

rpm 执行安装包二进制包（Binary）以及源代码包（Source）两种。二进制包可以直接安装在计算机中，而源代码包将会由RPM自动编译、安装。源代
码包经常以src.rpm作为后缀名。

注：查看E:\ReviewOfAllLearnedCourse\Summary\Useful_Skills\YunServer(Vultr)中mysql服务器的安装就是第二种，先安装源代码包，
再用yum安装软件。

常用命令组合(前两个最有用)：
－ivh：安装显示安装进度--install--verbose--hash        //i是install的意思，v是verbose(显示细节信息)的意思...
－Uvh：升级软件包--Update；
－qpl：列出RPM软件包内的文件信息[Query Package list]；
－qpi：列出RPM软件包的描述信息[Query Package install package(s)]；
－qf：查找指定文件属于哪个RPM软件包[Query File]；
－Va：校验所有的RPM软件包，查找丢失的文件[View Lost]；
－e：删除包
```

* wget（从指定的URL下载文件）
```
1、使用wget下载单个文件 
以下的例子是从网络下载一个文件并保存在当前目录 
wget http://cn.wordpress.org/wordpress-3.1-zh_CN.zip 
在下载的过程中会显示进度条，包含（下载完成百分比，已经下载的字节，当前下载速度，剩余下载时间）。 

2、使用wget -O下载并以不同的文件名保存 
wget默认会以最后一个符合”/”的后面的字符来命令，对于动态链接的下载通常文件名会不正确。 
错误：下面的例子会下载一个文件并以名称download.php?id=1080保存 
wget http://www.centos.bz/download?id=1 
即使下载的文件是zip格式，它仍然以download.php?id=1080命令。 
正确：为了解决这个问题，我们可以使用参数-O来指定一个文件名： 
wget -O wordpress.zip http://www.centos.bz/download.php?id=1080 

3、使用wget -c断点续传 
使用wget -c重新启动下载中断的文件: 
wget -c http://cn.wordpress.org/wordpress-3.1-zh_CN.zip 
对于我们下载大文件时突然由于网络等原因中断非常有帮助，我们可以继续接着下载而不是重新下载一个文件。需要继续中断的下载时可以使用-c参数

4、使用wget -b后台下载 
对于下载非常大的文件的时候，我们可以使用参数-b进行后台下载。 
wget -b http://cn.wordpress.org/wordpress-3.1-zh_CN.zip 
Continuing in background, pid 1840. 
Output will be written to `wget-log’. 
你可以使用以下命令来察看下载进度 
tail -f wget-log 

5、使用wget -i下载多个文件 
首先，保存一份下载链接文件 
cat > filelist.txt 
url1 
url2 
url3 
url4 
接着使用这个文件和参数-i下载 
wget -i filelist.txt 
```

#### Linux系统目录结构

/bin 二进制可执行命令

/dev 设备特殊文件 

/etc 系统管理和配置文件 

/etc/rc.d 启动的配置文件和脚本 

/home 用户主目录的基点，比如用户user的主目录就是/home/user，可以用`~user`表示 

/lib 标准程序设计库，又叫动态链接共享库，作用类似windows里的.dll文件 

/sbin 系统管理命令，这里存放的是系统管理员使用的管理程序 

/tmp 公用的临时文件存储点 

/root 系统管理员的主目录（呵呵，特权阶级） 

/mnt 系统提供这个目录是让用户临时挂载其他的文件系统。 

/lost+found 这个目录平时是空的，系统非正常关机而留下“无家可归”的文件（windows下叫什么.chk）就在这里

/proc 虚拟的目录，是系统内存的映射。可直接访问这个目录来获取系统信息。 

/var 某些大文件的溢出区，比方说各种服务的日志文件 

/usr 最庞大的目录，要用到的应用程序和文件几乎都在这个目录。其中包含： 

/usr/x11r6 存放x window的目录 
/usr/bin 众多的应用程序 
/usr/sbin 超级用户的一些管理程序 
/usr/doc linux文档 
/usr/include linux下开发和编译应用程序所需要的头文件 
/usr/lib 常用的动态链接库和软件包的配置文件 
/usr/man 帮助文档 
/usr/src 源代码，linux内核的源代码就放在/usr/src/linux里 
/usr/local/bin 本地增加的命令 
/usr/local/lib 本地增加的库