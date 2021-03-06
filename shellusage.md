bash shell 脚本执行的方法有多种，本文作一个总结，供大家学习参考。

    假设我们编写好的shell脚本的文件名为hello.sh，文件位置在/data/shell目录中并已有执行权限。

方法一：切换到shell脚本所在的目录（此时，称为工作目录）执行shell脚本：
cd /data/shell
./hello.sh
./的意思是说在当前的工作目录下执行hello.sh。如果不加上./，bash可能会响应找到不到hello.sh的错误信息。因为目前的工作目录（/data/shell）可能不在执行程序默认的搜索路径之列,也就是说，不在环境变量PASH的内容之中。查看PATH的内容可用 echo $PASH 命令。现在的/data/shell就不在环境变量PASH中的，所以必须加上./才可执行。

方法二：以绝对路径的方式去执行bash shell脚本：
/data/shell/hello.sh

方法三：直接使用bash 或sh 来执行bash shell脚本：
cd /data/shell

bash hello.sh
或
cd /data/shell

sh hello.sh
    注意，若是以方法三的方式来执行，那么，可以不必事先设定shell的执行权限，甚至都不用写shell文件中的第一行（指定bash路径）。因为方法三是将hello.sh作为参数传给sh(bash)命令来执行的。这时不是hello.sh自己来执行，而是被人家调用执行，所以不要执行权限。那么不用指定bash路径自然也好理解了啊，呵呵……。

方法四：在当前的shell环境中执行bash shell脚本：
cd /data/shell

. hello.sh
或
cd /data/shell

source hello.sh
    前三种方法执行shell脚本时都是在当前shell（称为父shell）开启一个子shell环境，此shell脚本就在这个子shell环境中执行。shell脚本执行完后子shell环境随即关闭，然后又回到父shell中。而方法四则是在当前shell中执行的。

假设shell脚本文件为hello.sh
放在/root目录下。

下面介绍几种在终端执行shell脚本的方法：
复制代码 代码如下:
[root@localhost home]# cd /root/
[root@localhost ~]#vim hello.sh
#!  /bin/bash
cd /tmp
echo "hello guys!"
echo "welcome to my Blog:linuxboy.org!"

1.切换到shell脚本所在的目录，执行：
复制代码 代码如下:
[root@localhost ~]# ./hello.sh
-bash: ./ hello.sh: 权限不够

2.以绝对路径的方式执行：
复制代码 代码如下:
[root@localhost ~]# /root/Desktop/hello.sh
-bash: /root/Desktop/ hello.sh: 权限不够

3.直接用bash或sh执行：
复制代码 代码如下:
[root@localhost ~]# bash hello.sh
hello guys!
welcome to my Blog:linuxboy.org!
[root@localhost ~]# pwd
/root
 
[root@localhost ~]# sh hello.sh
hello guys!
welcome to my Blog:linuxboy.org!
[root@localhost ~]# pwd
/root

   注意：用以上三种方法执行shell脚本，现行的shell会开启一个子shell环境，去执行shell脚本，前两种必须要有执行权限才能够执行。也可以让shell脚本在现行的shell中执行：

4.现行的shell中执行
复制代码 代码如下:
[root@localhost ~]# . hello.sh
hello guys!
welcome to my Blog:linuxboy.org!
[root@localhost tmp]# pwd
/tmp
 
[root@localhost ~]# source hello.sh
hello guys!
welcome to my Blog:linuxboy.org!
[root@localhost tmp]# pwd
/tmp

    对于第4种不会创建子进程，而是在父进程中直接执行。
    上面的差异是因为子进程不能改变父进程的执行环境，所以CD(内建命令，只有内建命令才可以改变shell 的执行环境)没有成功，但是第4种没有子进程，所以CD成功。
