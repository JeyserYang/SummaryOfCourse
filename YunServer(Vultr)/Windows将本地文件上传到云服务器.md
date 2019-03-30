#### 下载

可以从`http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html`下载pscp

#### 使用

* Windows-云服务器上传文件或目录

1)、开始→运行→cmd进入到dos模式，输入以下命令：

pscp E:\javaWP\new.txt root@130.75.7.156:/home/root    //下载文件
pscp E:\html root@45.76.4.198.83:/var/www/html/        //下载目录

2)、回车后，提示输入密码，在我们输入Linux服务器上该用户的登录密码后，E:\javaWP\new.txt这个文件会上传到 Linux 服务器的/home/root目录下。130.75.7.156是远程Linux服务器的IP。

* 云服务器-Windows下载文件或目录

1)、开始→运行→cmd进入到dos模式，输入以下命令：

pscp root@130.75.7.156:/home/root/new.txt E:\javaWP\new_copy.txt

2)、回车后，提示输入密码，输入密码后文件将上传到目标机器的/home/root目录下。

其中：root为linux的用户名，130.75.7.156为远程Linux主机ip地址，/home/root/new.txt为linux下的文件，E:\javaWP\new_copy.txt为保存在本地的文件。