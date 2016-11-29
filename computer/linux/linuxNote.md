# 一、系统安装

## 1、VMware网路设置

设置网络ifconfig eth0 192.168.9.21
使用windows的回环网卡，vmware网络设置
自定义特定虚拟网络（VMnet0自动桥接）

## 2、设置环境变量

### 2.1、修改/etc/profile

首先在此文件中设置环境变量;
export 设置好的环境变量.

```shell
vim /etc/profile

source /etc/profile
```

### 2.2、修改.bashrc

```shell
#vim /root/.bashrc 
export PATH="变量路径"
```

### 2.3、直接在shell下用export命令修改

```shell
#export PATH="$var_PATH" 
#export 可查看当前系统下的所有环境变量.
```

### 2.4、java环境变量示例

```shell
1. #修改java运行环境
2. export JAVA_HOME="xxx"
3. export PATH="$PATH:$JAVA_HOME/bin"
4. export JRE_HOME="$JAVA_HOME/jre"
5. export CLASSPATH=".:$JAVA_HOME/lib:$JRE_HOME/lib"
```

### 2.5、修改命令行显示

比较喜欢centos的命令行显示方式

可以在.bash_profile里加入如下配置

```shell
export PS1='[\u@\h \W]\$'
```

执行命令：

```shell
source .bash_profile
```



## 3、Ubuntu装机总结

### 3.1、安装系统到U盘

可以将Linux系统装到U盘或者是移动硬盘中。
如果使用VMware安装，那么比较简单，需要创建一个虚拟机，并且添加一块硬盘，硬盘就是我们想要安装系统的U盘或者是移动硬盘。
给新创建的虚拟机设置iso镜像。
直接启动按照提示，就可以将我们的系统装到U盘中了。

如果用电脑引导安装，需要使用UltraISO工具，将我们的系统镜像写入到一个U盘中。
然后设置U盘启动安装，原理是一样的。

### 3.2、安装OpenSSH服务

首先安装完ubuntu14之后，我们需要安装的第一个软件是openssh-server，这样就就可以是用远程终端操作Linux系统了。
更新源：
sudo apt-get update
安装openssh-server
sudo apt-get install openssh-server
使用ps -aux | grep ssh检查是否有sshd进程
有这个进程即可。

这时候就可以使用xshell远程连接了。

### 3.3、解决VI编辑器的上下左右变ABCD问题

Ubuntu  vi 上下左右变ABCD问题解决方法
错误问题：vi上下左右键显示为ABCD的问题
解决方法： 
只要依次执行以下两个命令即可完美解决Ubuntu下vi编辑器方向键变字母的问题。
　　1. 执行命令 sudo apt-get remove vim-common
　　2. 执行命令 sudo apt-get install vim

### 3.4、安装mysql

参考：http://blog.csdn.net/tianbiandeyun1116/article/details/7479115
安装前的准备，需要安装编译器

```
apt-get install g++ 
apt-cache search ncurses 
apt-get install libncurses5-dev 
```

备注：一台服务器安装多个mysql要注意几点: 
1.配置文件安装路径不能相同 
2.数据库目录不能相同 
3.启动脚本不能同名 
4.端口不能相同 
5.socket文件的生成路径不能相同 

mysql源码包安装过程： 

1下载mysql

```
wget http://ftp.jaist.ac.jp/pub/mysql/Downloads/MySQL-5.1/mysql-5.1.57.tar.gz 
```

2.创建mysql组和用户 

```
shell> su root 

shell> groupadd mysql 

shell> useradd -g mysql mysql 

shell> id mysql
```

3.创建安装目录 

```
mkdir -p /usr/local/mysql/data 

mkdir -p /usr/local/mysql/tmp 
```

4.编译安装mysql（经典的四步） 

```shell
tar -zxvf mysql-5.1.57.tar.gz 
cd mysql-5.1.57 
./configure --prefix=/usr/local/mysql --localstatedir=/usr/local/mysql/data --with-mysqld-user=mysql --with-charset=utf8 --with-unix-socket-path=/usr/local/mysql/tmp/mysql.sock
make 
make install 
```

5.修改权限和创建配置文件

``` shell
cp support-files/my-medium.cnf /usr/local/mysql1/my.cnf 
```

备注： 
根据机器配置的不同选择不同的文件： 
/user/local/mysql/share/mysql/my-small.cnf   最小配置安装，内存<=64M，数据数量最少 
/user/local/mysql/share/mysql/my-large.cnf 内存=512M 
/user/local/mysql/share/mysql/my-medium.cnf  32M<内存<64M，或者内存有128M，但是数据库与web服务器公用内存
/user/local/mysql/share/mysql/my-huge.cnf  1G<内存<2G，服务器主要运行mysql 
/user/local/mysql/share/mysql/my-innodb-heavy-4G.cnf  最大配置安装，内存至少4G

[mysqld] 
basedir = /usr/local/mysql1              定义mysql程序目录 
datadir = /usr/local/mysql1/data         定义数据目录 
并修改端口，各个mysql使用不同的端口 

设置权限

chown mysql:mysql /usr/local/mysql1/my.cnf 
chown -R mysql:mysql /usr/local/mysql1 

6.初始化mysql(用户表和权限表)和启动mysql 

进入mysql的安装目录

cd /usr/local/mysql1 

初始化数据库

```
root@pixboly:/usr/local/mysql# bin/mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --user=mysql
```

启动mysql

```shell
bin/mysqld_safe --defaults-file=/usr/local/mysql1/my.cnf & 
```

备注：&代表后台运行 
停止mysql可以使用：bin/mysqladmin
shutdown -u root -p 
设置root用户的密码 
bin/mysqladmin
-u root password '123456' 

备注：如果出现error:
'Can't connect to local MySQL server through socket '/tmp/mysql.sock'
(2)'解决办法：
ln
-s /usr/local/mysql1/tmp/mysql.sock /tmp/mysql.sock 
chmod
755 /tmp/mysql.sock 

\#登录mysql 
bin/mysql
-u root -p 

netstat-an | grep'3306'

至此安装就完成了，我装的非常成功![img](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAMAAADXqc3KAAABrVBMVEVdIwB8LQCKSguQTACuWgC4XwCuZg+pYhKwbhO8cQ2wcBq9dxSsbSWucCy9fy63ezKreVzDfA/LfgjBdBDDfRS2gUa4hEm5iFO7kWXOggvHghbBhhzLhxvLiRrajgvQhhTSihbSjRvdnB/DiCbLhyHMiyLLiCnCij7SjiHSljPWpDnklwrgnyTmqh/rpRbwowv0qAz7rwzxqxL9sg3zshf9tRD/vxnipCPkrCXprC3nvi/utyXvszD1tyn2vCb2ujXFnm3bpUPYsWvlvUTtuEvislLtvlj/whzuwC/rwDvuyj/2xSz9xCH5xS//yyb+zSr2yzb0zT/5xjH+0S3+1DP+2zruzkju0lv3wUX1wUv2xln3yFv+30H43Ezu1W/3ymf/40X+5Un/6Ez/41P/61L/6lv/8F3542L762//9W3/9nT/+HvLqoTIr5bhv4zSxbjmwIvu15n53Jvt0arz3rD/+Yf/+5X/+5r74aX85a///KD//bHX0czZ1M/c19Pn3tDj39zl4d7//sf//sj88Nbp5uPq6OXw7uz9+Ov08vH//Pb+/v78A/sAAAD///9Txn98AAAAjXRSTlP//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////wDYXlVrAAABj0lEQVR4nIXS90/CQBQHcHBhxUnce4viiufe+zQ0VLHnRO3BFS0gKlqraYNIJI7zj7Y94/rB+H75Ju+Tl7vcO9vbH2X7D17vjndn5vaOE4+/4XbOLyKERP/s6vUPeD3wS5gEg1hCiJ9Y/QK67yeEhMNhOYgDSBhe+YSEl2y3uNw7EcWM+imxd/EDnqcxaYrTePuN6o7TqyYfv3DPILGBiZNS6tQ1KzgB9i9SC5YRlrlSeymX1lkIPOjOWDAWIHKD3WbvTOtuM5q3+JH6JANzYqcgq/AyrR8VZOWvmRO1hgWtJiiqpuu6pioEIwGCcga9ooTDEfVG09SITCTRB4GTwYIXSXWHSjQaUWSCJ+sgGCxmcNEnBE65TdksjDdzTiBonGeHP3fwIjrlnOtYWi+z+gMl5+y6b/FxnyBKPbnZ2blLIQhAXnWSfjxiVx/0CTgkyyGr76o0Mp/76GodhtAL4SgY8TiqjNT3omJtNZ4hAIY8rqIz1v9a7UOsosjhKKyoNpIvvz8DfUoZZqUy//2Sd1SK35/2gXxmAAAAAElFTkSuQmCC)，啦啦啦～～～

备注： 
1、更改root用户的密码有两种方法(把root用户的密码从123456改成123456) 
(1)bin/mysqladmin -u root -p123456 password 123456 
(2)bin/mysql -u root -p 
  mysql> use mysql; 
  mysql> update user set password=password('654321') where user='root' 
  mysql>flush privileges; 

2、新增用户并授权 
 格式：grant select on 数据库.* to 用户名@登录主机 identified by "密码" 
 例如：grant all privileges on *.* to nova@'%' identified by '123456'; 
      grant select,insert,update,delete on mydb.* to test2@localhost   identified by "abc";
     grant all privileges on *.* to root@'%' with grant option

远程连接失败解决：

```
mysql> grant all privileges on *.* to 'root'@'%' identified by '123456'; 
mysql> flush privileges; 
mysql> exit
```

### 3.5、安装Tomcat

解压tomcat

```
tar -zxvf apache-tomcat-6.0.41.tar.gz
```

拷贝到安装目录

````
cp -r apache-tomcat-6.0.41 /usr/local/
````

启动tomcat

```shell
root@pixboly:/usr/local/apache-tomcat-6.0.41/bin# sh startup.sh 

Using CATALINA_BASE:   /usr/local/apache-tomcat-6.0.41

Using CATALINA_HOME:   /usr/local/apache-tomcat-6.0.41

Using CATALINA_TMPDIR: /usr/local/apache-tomcat-6.0.41/temp

Using JRE_HOME:        /usr/local/jdk1.8.0_112/jre

Using CLASSPATH:       /usr/local/apache-tomcat-6.0.41/bin/bootstrap.jar

root@pixboly:/usr/local/apache-tomcat-6.0.41/bin#
```

编辑/usr/local/apache-tomcat-6.0.41/conf/server.xml

将之前的8080改为80方便访问

```xml
<Connector port="80" protocol="HTTP/1.1"
```

关闭服务器重启服务器

```
root@pixboly:/usr/local/apache-tomcat-6.0.41/bin# sh shutdown.sh

root@pixboly:/usr/local/apache-tomcat-6.0.41/bin# sh startup.sh
```

### 3.6、安装sougou输入法

首先安装fcitx软件：

```Shell
sudo apt-get install fcitx
```

这个输入法可以支持搜狗

然后在System Setting -> Language Support里

Keyboard input method system:选fcitx

自己硬盘有搜狗的deb包，也可以下载最新的deb包。

```shell
dpkg -i xxxxx.deb
```

安装完成！

在Ubuntu14.01下可以直接点击下载的文件进入软件中心进行安装（这里的图是已经安装过的，没有安装过的按照Ubuntu的提示安装）.

![Ubuntu14.04安装搜狗输入法](http://b.hiphotos.baidu.com/exp/w=500/sign=7947e406710e0cf3a0f74efb3a46f23d/9213b07eca806538e5c2ed0794dda144ad3482fe.jpg)

接下来就是在终端中输入im-config，这时会出现一个对话框，点击OK，有一个对话框，点击Yes，你会看到下面的对话框。如果上面是fcitx，就不用管，直接关闭；如果不是，就修改上面的ibus为fcitx.点击OK即可。又会出现一个对话框，接着就是OK，最后重启电脑。

[![Ubuntu14.04安装搜狗输入法](http://g.hiphotos.baidu.com/exp/w=500/sign=ab617dcd3a01213fcf334edc64e636f8/dc54564e9258d10995afe0a6d258ccbf6c814d51.jpg)步骤阅读](http://jingyan.baidu.com/album/ad310e80ae6d971849f49ed3.html?picindex=3)

之后，在终端中输入：fcitx-config-gtk3出现对话框如下。点击对话框左下角的（+）按钮，弹出另一个对话框如上图所示。然后，取消Only Show Current Language（很重要，否则不能找到刚安装过的搜狗输入法!）最后，在输入框中输入sogou，选中点击OK即可。添加完后将其放置到列表的最下方，注意，是最下方！！！然后默认输入法就是搜狗输入法了。

[![Ubuntu14.04安装搜狗输入法](http://c.hiphotos.baidu.com/exp/w=500/sign=a7df81deac51f3dec3b2b964a4eff0ec/314e251f95cad1c89d4fb5337c3e6709c93d5129.jpg)步骤阅读](http://jingyan.baidu.com/album/ad310e80ae6d971849f49ed3.html?picindex=4)

### 3.7、虚拟机全屏

###　3.8、typora安装

typora是一个好用的markdown编辑器。安装方法访问官网

http://www.typora.io/#linux

这里写了一个脚本，进行安装，过程很慢，耐心等待。。。

```shell
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
sudo add-apt-repository 'deb https://typora.io ./linux/'
sudo apt-get update
sudo apt-get install typora
```



### 3.9、安装adobeflash-player 插件

```SHELL
sudo apt-get install flashplugin-installer
```

###　3.10、安装charles抓包工具

从官网下载即可：https://www.charlesproxy.com/

移动硬盘里有下好的！

### 3.11、Gvim

从ubuntu软件中心下载



### 3.12、安装SecureCRT

下载地址：http://pan.baidu.com/s/1ntqq6Op





# 二、常用命令

## 1、文件处理命令

### 1.1、显示目录文件

命令：ls 
原意：list 
选项：
-a显示所有文件，包括隐藏的
-l详细信息显示
-d查看目录属性
-i查看inode值 i节点

```shell
1. [root@localhost ~]# ls -al /
2. total 150
3. drwxr-xr-x  23 root root  4096 Oct  6 16:39 .
4. drwxr-xr-x  23 root root  4096 Oct  6 16:39 ..
5. -rw-r--r--   1 root root     0 Oct  6 16:38 .autofsck
6. drwxr-xr-x   2 root root  4096 Oct  6 09:59 bin
7. drwxr-xr-x   4 root root  1024 Oct  6 16:30 boot
8. drwxr-xr-x  11 root root  4400 Oct  6 08:41 dev
9. drwxr-xr-x  92 root root  4096 Oct  6 09:59 etc
10. drwxr-xr-x   3 root root  4096 Oct  6 16:37 home
11. drwxr-xr-x  13 root root  4096 Oct  6 09:59 lib
12. drwx------   2 root root 16384 Oct  6 16:21 lost+found
13. drwxr-xr-x   2 root root  4096 Jan 26  2010 media
14. drwxr-xr-x   2 root root     0 Oct  6 16:39 misc
15. drwxr-xr-x   3 root root  4096 Oct  6 16:40 mnt
16. drwxr-xr-x   2 root root     0 Oct  6 16:39 net
17. drwxr-xr-x   2 root root  4096 Oct  6 08:41 opt
18. dr-xr-xr-x 142 root root     0 Oct  6 16:37 proc
19. drwxr-x---   4 root root  4096 Oct  6 08:51 root
20. drwxr-xr-x   2 root root 12288 Oct  6 09:59 sbin
21. drwxr-xr-x   4 root root     0 Oct  6 16:37 selinux
22. drwxr-xr-x   2 root root  4096 Jan 26  2010 srv
23. drwxr-xr-x  11 root root     0 Oct  6 16:37 sys
24. drwxrwxrwt  15 root root  4096 Oct  6 09:57 tmp
25. drwxr-xr-x  14 root root  4096 Oct  6 16:24 usr
26. drwxr-xr-x  22 root root  4096 Oct  6 16:33 var
    范例：
27. drwxr-xr-x  14 root root  4096 Oct  6 16:24 usr
```

第一部分 文件信息

`drwxr-xr-x`

- d 目录Directory 
- -二进制文件 
- l 软链接文件Link
  第二部分 
  `14` 硬链接数
  第三部分 第四部分

`root root`
所有者 所属组
第五部分 

`4096` 
文件大小 
数据块 512字节
第六部分

`Oct  6 16:24`
最后修改时间

### 1.2、切换目录cd

命令：cd 
英文原意：change directory 
范例： 

cd / 切换到根目录 
cd .. 切换到上一级目录 

### 1.3、显示当前的目录pwd

命令：pwd 
英文原意：print working directory

### 1.4、创建目录mkdir

命令：mkdir 
英文原意：make directories

### 1.5、创建文件touch

命令：touch

### 1.6、复制操作cp

命令:cp 
英文原意：copy 
语法：cp -R [源文件或目录][目标目录] 
-R复制目录 
范例：复制文件，可以是多个

```shell
1. [root@localhost testlinux]# cp /etc/inittab /etc/services .
2. [root@localhost testlinux]# ll
3. total 372
4. -rw-r--r-- 1 root root   1666 Oct  6 17:45 inittab
5. -rw-r--r-- 1 root root 362031 Oct  6 17:45 services
6. [root@localhost testlinux]#
```

范例：复制目录

```shell
1. [root@localhost testlinux]# cp -R /etc .
2. [root@localhost testlinux]# ll
3. total 380
4. drwxr-xr-x 92 root root   4096 Oct  6 17:48 etc
5. -rw-r--r--  1 root root   1666 Oct  6 17:45 inittab
6. -rw-r--r--  1 root root 362031 Oct  6 17:45 services
7. [root@localhost testlinux]# 
```

### 1.7、剪切命令mv

命令：mv 
英文原意：move 
功能：移动文件、更名

### 1.8、删除操作rm

命令：rm 
英文原意：remove 
语法：rm -r 文件或目录 
-r 删除目录 
-f 强制删除

### 1.9、查看文件内容cat

命令：cat 
英文原意：concatenate and display files

### 1.10、分页显示文件内容more

命令：more 
语法：more [filename] 
(space)or f :pagedown 
enter :nextline 
q or Q :quit

### 1.11、查看文件前面的内容 head

命令：head 
语法：head -num[filename] 
-num line number 
范例：

```shell
1. [root@localhost testlinux]# head -5 /etc/services 
2. # /etc/services:
3. # $Id: services,v 1.42 2006/02/23 13:09:23 pknirsch Exp $
4. #
5. # Network services, Internet style
6. #
7. [root@localhost testlinux]# 
```

### 1.12、查看文件后面的内容tail

命令：tail 
语法：tail -num[filename] 
-num line number 
-f 动态显示文件后面的内容(用于观察实时查看文件内容) 
范例：

```shell
1. [root@localhost testlinux]# tail -5 /etc/services 
2. com-bardac-dw   48556/tcp                       # com-bardac-dw
3. com-bardac-dw   48556/udp                       # com-bardac-dw
4. iqobject        48619/tcp                       # iqobject
5. iqobject        48619/udp                       # iqobject
6. # Local services
7. [root@localhost testlinux]#
```

### 1.13、创建链接ln

命令：ln 
英文原意：link 
语法：ln -s [源文件][目标文件] 
-s soft 创建软链接
eg.

```shell
1. [root@localhost testlinux]# ln -s /etc/issue ./issue.soft
2. [root@localhost testlinux]# ls
3. issue.soft
4. [root@localhost testlinux]# ll /etc/issue ./issue.soft 
5. -rw-r--r-- 1 root root 47 Apr 25  2010 /etc/issue
6. lrwxrwxrwx 1 root root 10 Oct  6 18:13 ./issue.soft -> /etc/issue
7. [root@localhost testlinux]# 
```

软连接权限：lrwxrwxrwx 
软连接权限很大，但是操作时，操作的是源文件，用户操作软链接文件的权限取决于源文件权限。
时间值：Oct 6 18:13 
创建软链接时的时间值
eg.创建硬链接

```shell
1. [root@localhost testlinux]# ln /etc/issue ./issue.hard
2. [root@localhost testlinux]# ll /etc/issue ./issue.hard 
3. -rw-r--r-- 2 root root 47 Apr 25  2010 /etc/issue
4. -rw-r--r-- 2 root root 47 Apr 25  2010 ./issue.hard
5. [root@localhost testlinux]#
```

软链接文件和原文件所有值都相同，可以同步更新 
如果源文件被删除，软链接无法访问，而硬链接可以访问

硬链接和源文件有相同的inode所以可以同步更新

```shell
1. [root@localhost testlinux]# ls -i
2. 13074507 a       13074508 a.soft      13074506 issue.soft
3. 13074507 a.hard   6881473 issue.hard
4. [root@localhost testlinux]# 
```

注意：硬链接不能跨文件系统

### 1.14、查看文件大小及硬盘空间du、df

1. df 可以查看一级文件夹大小、使用比例、档案系统及其挂入点，但对文件却无能为力(只能查看windows下的类似C盘,D盘) 
   du 可以查看文件及文件夹的大小,比较好用
2. [ia@i5a6 ~]$ df -h 
   参数 -h 表示使用「Human-readable」的输出，也就是在档案系统大小使用 GB、MB 等易读的格式(比较使用的参数,比较你不行自己计算字节数)
3. du：查询文件或文件夹的磁盘使用空间 
   指定深入目录的层数,参数：–max-depth= 
   [ia@i5a6 ~]# du -h --max-depth=1 /usr/local/webserver/ 
   这两个指令基本上可以查看对应的文件的占用,达到清理的信息提供. 
   du：查询文件或文件夹的磁盘使用空间 
   如果当前目录下文件和文件夹很多，使用不带参数du的命令，可以循环列出所有文件和文件夹所使用的空间。这对查看究竟是那个地方过大是不利的，所以得指定深入目录的层数，参数：--max-depth=，这是个极为有用的参数！如下，注意使用“*”，可以得到文件的使用空间大小. 
   提醒：一向命令比linux复杂的FreeBSD，它的du命令指定深入目录的层数却是比linux简化，为 -d。
   以下是代码片段

```shell
1. [root@bsso yayu]# du -h --max-depth=1 work/testing
2. 27M     work/testing/logs
3. 35M     work/testing
4. [root@bsso yayu]# du -h --max-depth=1 work/testing/*
5. 8.0K    work/testing/func.php
6. 27M     work/testing/logs
7. 8.1M    work/testing/nohup.out
8. 8.0K    work/testing/testing_c.php
9. 12K     work/testing/testing_func_reg.php
10. 8.0K    work/testing/testing_get.php
11. 8.0K    work/testing/testing_g.php
12. 8.0K    work/testing/var.php
13. [root@bsso yayu]# du -h --max-depth=1 work/testing/logs/
14. 27M     work/testing/logs/
1. [root@bsso yayu]# du -h --max-depth=1 work/testing/logs/*
2. 24K     work/testing/logs/errdate.log_show.log
3. 8.0K    work/testing/logs/pertime_show.log
4. 27M     work/testing/logs/show.log
```

## 2、权限处理命令

### 2.1、改变文件权限chmod

命令：chmod 
英文原意:change the permissions mode of a file 
语法：chmod [{ugo}{+/-/=}{rwx}][file or directory] 
chmod [mode=421] [file or directory]
r = 4
w = 2
x = 1

| 代表字符 | 权限   | 对文件的含义   | 对目录的含义       |
| ---- | ---- | -------- | ------------ |
| r    | 读权限  | 可以查看文件内容 | 可以列出目录中的内容   |
| w    | 写权限  | 可以修改文件内容 | 可以在目录中创建删除文件 |
| x    | 执行权限 | 可以执行文件   | 可以执行文件       |

文件
r cat\more\head\tail
w echo\vi
x 命令、脚本

目录
r ls
w touch\vi\rm\mkdir
x cd

### 2.2、改变文件所有者chown

命令：chown 
英文原意：change file ownership 
format:

```
chown [user] [file/directory]
```

eg.

```
1. [root@localhost testlinux]# ls -al a
2. -rwxrwxrwx 2 root root 0 Oct  6 18:27 a
3. [root@localhost testlinux]# chown centos a
4. [root@localhost testlinux]# ls -al a
5. -rwxrwxrwx 2 centos root 0 Oct  6 18:27 a
6. [root@localhost testlinux]#  
```

### 2.3、改变所属组chgrp

命令：chgrp 
英文原意：change file group ownership 
语法： 

```e
chgrp [用户组][file or directory]
```

eg.

```
1. [root@localhost testlinux]# ls -al a
2. -rwxrwxrwx 2 centos root 0 Oct  6 18:27 a
3. [root@localhost testlinux]# chgrp adm a
4. [root@localhost testlinux]# ls -al a
5. -rwxrwxrwx 2 centos adm 0 Oct  6 18:27 a
6. [root@localhost testlinux]# 
```

### 2.4、掩码值umask

创建文件和目录都有一个默认的权限

```
1. [root@localhost testlinux]# umask
2. 0022
3. [root@localhost testlinux]# umask -S
4. u=rwx,g=rx,o=rx
5. [root@localhost testlinux]# 
```

0022
0:特殊权限位
022：用户权限位，权限掩码值 
777 - 755 = 022 
linux权限规则： 
缺省创建文件不能赋予可执行权限 
所以创建目录权限是755 
而创建文件权限是 644
改变文件系统缺省掩码值为750

```
1. umask 027
```

## 3、用户管理

### 3.1、增加用户useradd

命令：useradd

```
1. [root@localhost testlinux]# useradd pix
2. [root@localhost testlinux]#
```

### 3.2、为用户设置密码passwd

命令：passwd
e.g. 

```
1. [root@localhost testlinux]# passwd pix
2. Changing password for user pix.
3. New UNIX password: 
4. Retype new UNIX password: 
5. passwd: all authentication tokens updated successfully.
6. [root@localhost testlinux]#

```

## 4、文件搜索

### 4.1、命令搜索which

命令:which 
格式:which [command name]
eg.查找chmod命令

```
1. [root@localhost testlinux]# which chmod
2. /bin/chmod
3. [root@localhost testlinux]#
```

### 4.2、命令搜索2 whereis

命令：whereis 
说明： 
和which区别很小，都可以显示命令的绝对路径 
which可以找到命令的别名记录 
whereis 可以找到帮助文档所在目录

```java
1. [root@localhost testlinux]# which ls
2. alias ls='ls --color=tty'
3.         /bin/ls
4. [root@localhost testlinux]# whereis ls
5. ls: /bin/ls /usr/share/man/man1p/ls.1p.gz /usr/share/man/man1/ls.1.gz
6. [root@localhost testlinux]#
```

### 4.3、文件搜索命令find

命令：find 
语法：find [search directory] [关键字]
**-name选项 按名称查找**

```
1. [root@localhost testlinux]# find /etc/ -name init
2. /etc/sysconfig/init
3. [root@localhost testlinux]# 
```

只会匹配和文件名相同的结果

通配符*和？

eg.查找所有以init开头的文件

```
1. [root@localhost testlinux]# find /etc/ -name init*
2. /etc/sysconfig/init
3. /etc/sysconfig/network-scripts/init.ipv6-global
4. /etc/rc.d/init.d
5. /etc/initlog.conf
6. /etc/init.d
7. /etc/inittab
8. /etc/selinux/targeted/contexts/initrc_context
9. [root@localhost testlinux]# 
```

?通配符

```
1. [root@localhost testlinux]# find /etc -name init???
2. /etc/inittab
3. [root@localhost testlinux]# 
```

**-size 根据文件大小来查找 **
以数据块为单位512字节 
如>100M的文件 
100M = 102400k = 204800block
大于 +204800 >204800 
小于 -204800 <204800
eg、查找系统中大于100M的文件

```
1. [root@localhost testlinux]# find / -size +204800
2. /sys/devices/pci0000:00/0000:00:0f.0/resource1
3. /proc/kcore
4. find: /proc/15122/task/15122/fd/4: No such file or directory
5. find: /proc/15122/fd/4: No such file or directory
6. [root@localhost testlinux]#
```

**-user	根据所有者来查找**

```
1.  [root@localhost testlinux]# find /home -user centos
2. /home/centos
3. /home/centos/.nautilus
4. /home/centos/.nautilus/metafiles
5. /home/centos/.nautilus/metafiles/x-nautilus-desktop:%2F%2F%2F.xml
6. /home/centos/.bash_profile
7. /home/centos/.dmrc
8. /home/centos/.eggcups
9. /home/centos/.gconf
10. /home/centos/.gconf/apps
11. /home/centos/.gconf/apps/panel
12. /home/centos/.gconf/apps/panel/applets
13. /home/centos/.gconf/apps/panel/applets/clock
```

- 以天为单位-ctime、-atime、-mtime 根据时间查找
- 以分钟为单位-cmin、-amin、-mmin
- c-change改变，表示文件的属性被修改
- a-access访问
- m-modify修改，表示文件的内容被修改

-多长时间之内 
+超过

eg、查找home目录下，两个小时之内被修改过文件内容的文件

```
1. [root@localhost testlinux]# find /home -mmin -120
2. /home
3. /home/centos/test/testlinux
4. /home/centos/test/testlinux/a.hard
5. /home/centos/test/testlinux/issue.soft
6. /home/centos/test/testlinux/a.soft
7. /home/centos/test/testlinux/a
8. /home/centos/.xsession-errors
9. /home/pix
10. /home/pix/.bash_profile
11. /home/pix/.bashrc
12. /home/pix/.mozilla
13. /home/pix/.mozilla/extensions
14. /home/pix/.mozilla/plugins
15. /home/pix/.bash_logout
16. [root@localhost testlinux]
```

**连接符**
-a：逻辑与
-o:逻辑或
eg、查找系统中大于80M小于100M的文件

```
1. find /etc -size +163840 -a -size -204800
```

-type 文件类型查找 
-f:二进制文件 
-l:软链接文件 
-d:目录文件
链接执行符

```
1.  find ..... -exec 命令 {} \; 
```

也可以用ok代替exec，询问是否执行
{}find查询的结果 
\转义符 
;结束

eg.

```
1. [root@localhost testlinux]# find /etc -name inittab -exec ls -ls {} \;
2. 8 -rw-r--r-- 1 root root 1666 Oct  6 08:41 /etc/inittab
3. [root@localhost testlinux]# 
```

根据i节点查找

```
1. [root@localhost testlinux]# ls -i
2. 13074507 a       13074508 a.soft      13074506 issue.soft
3. 13074507 a.hard   6881473 issue.hard  13074516 newfile
4. [root@localhost testlinux]# find -inum 13074516
5. ./newfile
6. [root@localhost testlinux]#
```

根据i节点删除文件

```
localhost testlinux]# ls
8. a  a.hard  a.soft  issue.hard  issue.soft  newfile
9. [root@localhost testlinux]# find -inum 13074516 -exec rm {} \;
10. [root@localhost testlinux]# ls
11. a  a.hard  a.soft  issue.hard  issue.soft
12. [root@localhost testlinux]# 
```

### 4.4、locate查找

命令：locate 
英文原意：list files in datebases

### 4.5、更新文件信息数据库

命令：updatedb 
英文原意：update the slocate datebase 
功能：建立整个系统目录文件的数据库
两个命令配合使用,updatedb和locate

```
1. [root@localhost testlinux]# updatedb
2. [root@localhost testlinux]# locate inittab
3. /etc/inittab
4. /usr/share/man/man5/inittab.5.gz
5. /usr/share/terminfo/a/ansi+inittabs
6. [root@localhost testlinux]# 
```

先用updatedb更新数据库 
再用locate查找，速度快

4.7、文件搜索命令grep
Format：grep [key string] [src file]
列出内容包含行 
范例：

```
1. [root@localhost testlinux]# grep centos /etc/passwd
2. centos:x:500:500:centos:/home/centos:/bin/bash
3. [root@localhost testlinux]#

```

## 5、帮助命令

### 5.1、man帮助

名称：man 
英文原意：manual 
Format：man [命令或配置文件]

1. man 1 passwd 命令的帮助
2. man 5 passwd 配置文件帮助

### 5.2、info帮助

命令：info 
英文原意：information 
范例 
info ls

### 5.3、只看命令作用帮助

命令：whatis
范例： 
whatis ls

### 5.4、看命令选项

ls --help

### 5.5、查看服务相关文件

命令:apropos 
列出所有相关文件 
apropos services = man -k services

### 5.6、更新帮助数据库

命令：makewhatis 
给whatis和apropos更新最新信息

### 5.7、查看系统内置命令帮助

命令：help
help cd

## 6、压缩解压命令

### 6.1、gz压缩

命令：gzip 
英文原意：GUN zip 
Format：gzip 选项 [filename] 
压缩后格式：.gz 
特点：只能压缩文件，不保留源文件
范例： 

```
[root@localhost testlinux]# touch newfile 
[root@localhost testlinux]# gzip newfile 
[root@localhost testlinux]# ls 
a a.hard a.soft issue.hard issue.soft newfile.gz 
[root@localhost testlinux]# 
```

### 6.2、gz解压缩

命令：gunzip 
英文原意：GUN unzip 
语法：gunzip 选项[压缩文件 
范例：gunzip file.zip 
gunzip = gzip -d

### 6.3、tar.gz压缩

命令：tar 
语法：tar 选项[cvf][目录] 
-c：产生.tar打包文件 create 
-v:显示详细信息 
-f:指定压缩后的文件名 
-z:打包同时压缩 
功能描述：打包目录 
压缩后格式：.tar.gz
eg. 一步完成

```
1. [root@localhost testlinux]# ls
2. a  a.hard  a.soft  issue.hard  issue.soft  newdir  newfile
3. [root@localhost testlinux]# tar -zcvf newdir.tar.gz newdir
4. newdir/
5. [root@localhost testlinux]# ls
6. a  a.hard  a.soft  issue.hard  issue.soft  newdir  newdir.tar.gz  newfile
```

eg.两步完成

```
1. [root@localhost testlinux]# ls
2. a       a.soft      issue.soft  newdir2        newfile
3. a.hard  issue.hard  newdir      newdir.tar.gz
4. [root@localhost testlinux]# tar -cf newdir2.tar newdir2/
5. [root@localhost testlinux]# ls
6. a       a.soft      issue.soft  newdir2      newdir.tar.gz
7. a.hard  issue.hard  newdir      newdir2.tar  newfile
8. [root@localhost testlinux]# gzip newdir2.tar 
9. [root@localhost testlinux]# ls
10. a       a.soft      issue.soft  newdir2         newdir.tar.gz
11. a.hard  issue.hard  newdir      newdir2.tar.gz  newfile
12. [root@localhost testlinux]#
```

### 6.4、tar.gz解压缩

tar命令解压缩语法： 
-x 解包.tar文件 
-v 显示详细信息 
-f 指定解压文件 
-z 解压缩
范例： 

tar -zxvf dir1.tar.gz 

### 6.5、zip压缩

命令：zip 
语法：zip 选项[-r][压缩后文件名] [文件或目录] 
-r 压缩目录
范例：

```
1. [root@localhost testlinux]# zip new.zip new
2.   adding: new (stored 0%)
3. [root@localhost testlinux]# ls
4. a       a.soft      issue.soft  newdir   newdir2.tar.gz  newfile
5. a.hard  issue.hard  new         newdir2  newdir.tar.gz   new.zip
6. [root@localhost testlinux]#
```

压缩目录

```
1. [root@localhost testlinux]# zip -r newdir.zip newdir
2.   adding: newdir/ (stored 0%)
3. [root@localhost testlinux]# ls
4. a       a.soft      issue.soft  newdir   newdir2.tar.gz  newdir.zip  new.zip
5. a.hard  issue.hard  new         newdir2  newdir.tar.gz   newfile
6. [root@localhost testlinux]#
```

### 6.6、zip解压缩

命令：unzip 
Format：unzip [filename.zip]

```
1. [root@localhost testlinux]# unzip newdir.zip 
2. Archive:  newdir.zip
3. [root@localhost testlinux]# ls
4. a       a.soft      issue.soft  newdir   newdir2.tar.gz  newdir.zip  new.zip
5. a.hard  issue.hard  new         newdir2  newdir.tar.gz   newfile
6. [root@localhost testlinux]#
```

### 6.7、bz2压缩

命令：bzip2 
Format：bzip2 选项 [-k][文件名] 
-k 是否保留源文件 
压缩后格式：.bz2 
范例：

```
1. [root@localhost testlinux]# bzip2 -k newfile5 
2. [root@localhost testlinux]# ls
3. a       issue.hard  newdir          newdir.tar.gz  newfile5
4. a.hard  issue.soft  newdir2         newdir.zip     newfile5.bz2
5. a.soft  new         newdir2.tar.gz  newfile        new.zip
6. [root@localhost testlinux]#
```

### 6.8、bz2解压缩

命令：bunzip2 
Format:bunzip2 选项 [-k][filename.bz2] 
-k 解压缩后是否保留原文件 
范例：

```
1. [root@localhost testlinux]# bunzip2 -k newfile5.bz2 
2. [root@localhost testlinux]# ls
3. a       issue.hard  newdir          newdir.tar.gz  newfile5
4. a.hard  issue.soft  newdir2         newdir.zip     newfile5.bz2
5. a.soft  new         newdir2.tar.gz  newfile        new.zip
6. [root@localhost testlinux]#
```

## 7、网络通信命令

### 7.1、向指定用户发消息write

命令：write 
Format：write <用户名> 
description： Send a message to a user,ctrl+D end.

```
1. [root@localhost pix]# write root
2. write: root is logged in more than once; writing to pts/2
3. hello root!
4. [root@localhost pix]# 
```

### 7.2、向所有用户发广播wall

command:wall 
Format:wall [message][filename] 
Description:Send a broadcast to all user. 
eg.

```
1. [root@localhost pix]# wall Happy new year!
2. 
3. Broadcast message from root (pts/3) (Fri Oct  7 04:08:20 2016):
4. 
5. Happy new year!
6. [root@localhost pix]# 
```

### 7.3、测试网络联通性

Command：ping 
Format：ping selection IPAddress. 
selection: 
-c:times 
-s:send packet size(<65507) 
eg.

```
1. [root@localhost ~]# ping -c 4 127.0.0.1
2. PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
3. 64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.197 ms
4. 64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.038 ms
5. 64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.039 ms
6. 64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.039 ms
7. 
8. --- 127.0.0.1 ping statistics ---
9. 4 packets transmitted, 4 received, 0% packet loss, time 3002ms
10. rtt min/avg/max/mdev = 0.038/0.078/0.197/0.068 ms
11. [root@localhost ~]#
```

### 7.4、网络通信命令ifconfig

Command:ifconfig 
Format:ifconfig selectin[-a][网卡设备标识] 
-a:显示所有网卡信息 
Description：Look net setting infomation! 
eg.

```java
1. [root@localhost ~]# ifconfig -a
2. eth0      Link encap:Ethernet  HWaddr 00:0C:29:1D:62:A2  
3.           inet addr:192.168.9.21  Bcast:192.168.9.255  Mask:255.255.255.0
4.           inet6 addr: fe80::20c:29ff:fe1d:62a2/64 Scope:Link
5.           UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
6.           RX packets:8445 errors:0 dropped:0 overruns:0 frame:0
7.           TX packets:7669 errors:0 dropped:0 overruns:0 carrier:0
8.           collisions:0 txqueuelen:1000 
9.           RX bytes:722150 (705.2 KiB)  TX bytes:1334834 (1.2 MiB)
10.           Interrupt:67 Base address:0x2000 
11. 
12. lo        Link encap:Local Loopback  
13.           inet addr:127.0.0.1  Mask:255.0.0.0
14.           inet6 addr: ::1/128 Scope:Host
15.           UP LOOPBACK RUNNING  MTU:16436  Metric:1
16.           RX packets:332 errors:0 dropped:0 overruns:0 frame:0
17.           TX packets:332 errors:0 dropped:0 overruns:0 carrier:0
18.           collisions:0 txqueuelen:0 
19.           RX bytes:35373 (34.5 KiB)  TX bytes:35373 (34.5 KiB)
20. 
21. sit0      Link encap:IPv6-in-IPv4  
22.           NOARP  MTU:1480  Metric:1
23.           RX packets:0 errors:0 dropped:0 overruns:0 frame:0
24.           TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
25.           collisions:0 txqueuelen:0 
26.           RX bytes:0 (0.0 b)  TX bytes:0 (0.0 b)
27. 
28. [root@localhost ~]# 
```

范例：改变IP

1. ifconfig eth0 192.168.9.21

## 8、系统关机命令

### 8.1、关机

Command:shutdown 
Format:shutdown -h now立刻关机

### 8.2、重启

Command：reboot

## 9、shell应用技巧

### 9.1、命令补全

TAB key 补全
history查看历史 ↑ 和 ↓滚动历史命令
clear清屏 ctrl + l
清除当前行ctrl + u

### 9.2、命令别名alias

命令别名定义： 
范例：

1. alias copy=cp
2. alias xrm=rm -r
   查看别名信息：alias 
   删除别名：unalias copy



### 9.3、输入输出重定向 > >> <

同标准I/O一样，Shell对于每一个进程预先定义3个文件描述符（0、1、2）。分别对应于：
0 （STDIN）标准输入
1 （STDOUT）标准输出
2 （STDERR）标准错误输出
输出重定向 > 或 >>

1. ls -l /tmp > /tmp.msg
2. date >> /tmp.msg
   输入重定向 <
3. wall < /etc/motd
   错误输出重定向 2>
4. cp -R /usr/backup/usr.bak 2> /bak.error
   9.4、管道
   管道：将一个命令的输出传送给另一个命令，作为另一个命令的输入 
   使用方法： 
   命令1 | command2 | CMD3 | CMD4 ...|CMD N 
   范例：
5. ls -l /etc | more
6. ls -l /etc | grep init
7. ls -l /etc | grep init | wc -l



### 9.5、命令链接符

；命令顺序执行，一个一个的执行。
&& 前后命令逻辑与关系，前面命令执行成功，后面命令才会执行。前面出错，后面不执行。
|| 前后命令逻辑或关系。前面成功，后面不执行，前面不成功，后面执行。

### 9.6、命令替换

将一个命令的输出作为另一个命令的参数。 
格式为：

````
命令1 `命令2` 
````

范例

```
ls -l `which touch`
```



# 三、VI编辑器

## 1、复制一段代码

使用vim有时需要移动一大段代码，以前都是在gedit里复制粘贴，今天找了一下，方法如下：
复制特定的某一段：把光标移到要复制的文本的头部，按下“v”，往后移动光标，光标所过之处的字符>都会高亮，移到欲复制文本的尾部后，按下“y”，高亮文本全部被复制到剪粘板。按下“p”粘贴到目的地。
剪切特定的某一段：把光标移到要剪切的文本的头部，按下“v”，往后移动光标，光标所过之处的字符>都会高亮，移到欲剪切文本的尾部后，按下“d”，高亮文本全部被复制到剪粘板。按下“p”粘贴到目的地。

## 2、Vim配置

[http://blog.163.com/023_dns/blog/static/1187273662012125112426472/] 
[http://blog.csdn.net/cbffyx/article/details/8998238]

### 2.1、打开高亮

[syntax on]：vi编辑器默认不打开语法加亮功能，打开vi编辑器后在[last line mode]下使用[syntax on]命令即可打开语法加亮功能，此时编辑器会高亮显示文件中的关键字，方便编程使用，用[syntax off]命令可关闭该功能。

## 3、工作模式

命令模式：保存，推出
插入模式:iao
编辑模式: ":" 命令回车结束运行

## 4、插入命令

| **命令** | **作用**               |
| :----- | :------------------- |
| a      | 光标后附加文字              |
| A      | 本行行行末出               |
| i      | 光标前出入文本              |
| I      | line				start insert |
| o      | 光标下插入新行              |
| O      | 光标上出入新行              |



## 5、定位命令

| **命令** | **作用**  |
| ------ | ------- |
| h、方向左键 | 左移动一个字符 |
| j、方向下键 | 下移动一行   |
| k、方向上键 | 上移动一行   |
| l、方向右键 | 右移动一个字符 |
| $      | 移至行尾    |
| 0      | 移至行首    |
| H      | 移至屏幕上端  |
| M      | 移至屏幕中央  |
| L      | 移至屏幕下端  |



| **命令**      | **作用** |
| ----------- | ------ |
| set				nu   | 设置行号   |
| set				nonu | 取消行号   |
| gg          | 到第一行   |
| G           | 到最后一行  |
| nG          | 到第n行   |
| :n          | 到第n行   |



## 6、删除命令

| **命令**  | **作用**     |
| ------- | ---------- |
| x       | 删除光标所在处的字符 |
| nx      | 删除光标后第n个字符 |
| dd      | 删除光标所在行    |
| ndd     | 删除光标后n行    |
| D       | 删除光标所在处到行尾 |
| :n1,n2d | 删除指定范围行    |



## 7、复制剪切命令

| **命令** | **作用**    |
| ------ | --------- |
| yy、Y   | 复制当前行     |
| nyy、nY | 复制当前行以下n行 |
| dd     | 剪切当前行     |
| ndd    | 剪切当前行以下n行 |
| p      | 粘帖到当前行下   |
| P      | 粘帖当当前行上   |

## 8、替换和取消命令

| **命令** | **作用**              |
| ------ | ------------------- |
| r      | 取代光标所在处字符           |
| R      | 从光标所在处开始替换字符，按ESC结束 |
| u      | 取消上一步操作             |

## 9、搜索和替换命令

| **命令**            | **作用**                 |
| ----------------- | ---------------------- |
| /string           | 向前搜索字符串                |
|                   | 搜索时忽略大小写：set				ic     |
|                   | 取消搜索时忽略大小写：set				noic |
| n                 | 搜索时指定搜索的下一个出现位置        |
| N                 | 搜索时指定搜索的上一个出现位置        |
| :%s/old/new/g     | 全文替换指定字符串              |
| :n1,n2s/old/new/g | 在一定范围内替换指定字符串          |
|                   | 把g换成c提示是否替换            |

## 10 、导入文件

命令::r 文件名 
范例:

1. :r /etc/issue

## 11、Vi中执行命令

命令：!命令 
范例：在vi中命令模式下，查看home目录有那些文件

1. :!ls /home
   范例：导入命令和执行命令组合使用 
   将date命令执行结果导入
2. :r !date

## 12、定义快捷键map

格式：map 快捷键 触发的命令 
注意：^这个符号，是这样按出来的，ctrl+v ctrl+p或者ctrl+v+p
范例：快速注释一行

1. :map ^P I#<ESC>
   范例：快速取消注释
2. map ^B 0x
   范例：出入邮箱地址
3. :map itpxsky@163.com
   范例：连续行注释
4. :n1,n2s/^/#/g     //^表示行首，就是在n1到n2行首加#
   范例：连续行去掉注释
5. :n1,n2s/^#//g    //去掉行首#
   范例：加//的注释
6. :n1,n2s/^/\/\//g 

# 四、用户管理

## 1、配置文件

### 1.1、用户信息文件

用户信息文件:/etc/passwd

```
1. gdm:x:42:42::/var/gdm:/sbin/nologin
2. centos:x:500:500:centos:/home/centos:/bin/bash
3. pix:x:501:501::/home/pix:/bin/bash
4. //用户名:密码位:UID:GID:描述信息:宿主目录:shell
```

查看passwd的帮助

1. man 5 passwd
   格式

| **字段** | **含义**             |
| ------ | ------------------ |
| 用户名    | 用户登录系统时使用的用户名      |
| 密码     | 密码位                |
| UID    | 用户标识号              |
| GID    | 缺省组标识号             |
| 注释性描述  | 例如存放用户全名等信息        |
| 宿主目录   | 用户登录系统后的缺省目录       |
| 命令解释器  | 用户使用的shell，默认为bash |

用户类型

**超级用户**（root，UID=0）
**普通用户**(UID500-60000)
**伪用户**(UID1-499) 
不是只有root才是超级管理员，是UID=0的用户是超级管理员
伪用户
bin，deamon，shutdown，halt等，任何Linux系统都默认有这些伪用户 
mail，news，games，apache，ftp，mysql，sshd，与Linux系统的进程相关。 
伪用户通常无法登录或不需要登录系统 
可以没有宿主目录
**用户组**
每个用户都属于一个用户组
每个用户组可以包括多个用户
同一个用户组的用户享有

### 1.2、密码文件

用户密码文件:/etc/shadow 
影子文件

```
1. gdm:!!:17080:0:99999:7:::
2. centos:$1$DCIybIGu$XOV.JIl3aPNPcfwqRlntK.:17080:0:99999:7:::
3. pix:$1$qUaLsnUn$bjU24LMH8NeEjIQGs.PKF.:17081:0:99999:7:::
4. root:$1$3NtuPFIJ$Og4CALiMzs6jdaaBYV.vv0:17080:0:99999:7:::
5. //用户名:加密密码:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:
```

pwunconv命令，把密码回写，就是把/etc/shadow去掉，将密码保存到/etc/passwd中
pwconv将密码转到/etc/shadow中

| **字段**   | **含义**          |
| -------- | --------------- |
| 用户名      | 用户登录系统时使用的用户用户名 |
| 密码       | 加密密码            |
| 最后一次修改时间 | 用户最后一次修改密码的天数   |
| 最小时间间隔   | 两次修改密码之间的最小天数   |
| 最大时间间隔   | 密码保持有效的最多天数     |
| 警告时间     | 从系统警告到密码失效的天数   |
| 帐号闲置时间   | 帐号闲置时间          |
| 失效时间     | 密码失效的绝对天数       |
| 标志       | 一般不用            |

如果密码忘记了，那么只要把/etc/shadow下对应用户的密码删掉即可
范例：手工创建一个用户

```javascript
1. //1、向/etc/passwd添加用户信息，新增一条
2. vi /etc/passwd
3. hosander::502:502:prject zhangxiaoguang:/home/hosander:/bin/bash
4. //2、创建宿主目录
5. mkdir /home/hosander
6. //3、为宿主目录改变权限
7. chown hosander /home/hosander
8. //4、编辑密码文件/etc/shadow
9. vi /etc/shadow
10. hosander:: ...后面无所谓复制一份普通用户的
11. //5、拷贝新用户配置文件/etc/skel
12. cp -f /etc/skel/.* /home/hosander
13. cp -rf /etc/skel/.mozilla/ /home/hosander/
```

### 1.3、用户组文件

用户组文件:/etc/group

### 1.4、用户组密码文件

用户组密码文件/etc/gshadow

### 1.5、用户配置文件

/etc/login.defs 用户登录缺省配置文件 
/etc/default/useradd

```
1. [root@localhost ~]# cat /etc/default/useradd 
2. # useradd defaults file
3. GROUP=100
4. HOME=/home
5. INACTIVE=-1
6. EXPIRE=
7. SHELL=/bin/bash
8. SKEL=/etc/skel
9. CREATE_MAIL_SPOOL=yes
10. 
11. [root@localhost ~]# 
```

### 1.6、新用户信息文件

/etc/skel

### 1.7、登录信息

/etc/motd 登录成功后提示信息！ 
message of the today
/etc/issue登录时提示信息

## 2、文件的特殊权限

### 2.1、SetUID

普通用户为什么可以改密码? 
思考：普通用户对/etc/passwd和/etc/shadow文件都没有写权限为什么可以改密码?

```
1. [centos@localhost root]$ ls -l /etc/passwd /etc/shadow
2. -rw-r--r-- 1 root root 1719 Oct 12 07:43 /etc/passwd
3. -r-------- 1 root root 1121 Oct 12 07:44 /etc/shadow
4. [centos@localhost root]$ 
```

这是由于改密码的命令有s权限！

```
1. [centos@localhost root]$ ls -l /usr/bin/passwd
2. -rwsr-xr-x 1 root root 22984 Jan  6  2007 /usr/bin/passwd
3. [centos@localhost root]$
```

**SetUID**
定义：当一个可执行程序具有SetUID权限，用户执行这个程序时将以这个程序所有者的身份执行。
设置程序的SetUID属性

```
1. chmod u+s filename
2. 或
3. chmod 4755 filename
```

取消SetUID属性

```
1. chmod u-s filename
2. 或
3. chmod 755 filename
```

观察给一个普通文件设置SetUID属性

```
1. [root@localhost testlinux]# ls -l newfile
2. -rw-r--r-- 1 root root 0 Oct  6 21:22 newfile
3. [root@localhost testlinux]# chmod u+s newfile
4. [root@localhost testlinux]# ls -l newfile
5. -rwSr--r-- 1 root root 0 Oct  6 21:22 newfile
6. [root@localhost testlinux]# 
```

写权限位置变成了大写的S，普通文件newfile不是一个可执行程序所以加上SetUID属性就没有任何的意义。

```
umask 0022 
SetUID = 4
```

范例：touch命令授予SetUID属性 
当使用touch命令创建一个文件，所有者是创建文件的用户。 
当SetUID的属性赋予touch命令，这时无论谁使用touch命令创建文件，所有者都会变为root
不要随便给程序设置SetUID属性，如vi,用户管理很危险

### 2.2、SetGID

和SetUID同理 
设置组ID

```
1. chmod g+s filename
2. or
3. chmod 2755 filename
```

取消组ID

```
1. chmod g-s filename
2. or
3. chmod 755 filename
```

设置UID+GID

```
1. chmod 6755 filename
```

### 2.3、粘着位

如果一个权限为777的目录，被设置了粘着位，每个用户的可以在这个目录中创建文件，但只可以删除自己是所有者的文件（只可以删除自己创建的文件，删除不了别人的文件）
给目录增加粘着位

```
1. chmod o+t directory
2. or
3. chmod 1777 directory
```

Linux系统的/tmp目录就是这样的

```
1. [root@localhost testlinux]# ls -ld /tmp
2. drwxrwxrwt 15 root root 4096 Oct 12 07:00 /tmp
3. [root@localhost testlinux]#
```

标志other属性的执行目录有t的标志

### 2.4、搜索指定权限的文件

```
1. find directory -perm -xxxx
```

范例:查找所有具有SetGID的文件

```
1. [root@localhost testlinux]# find / -perm -4000
2. /lib/dbus-1/dbus-daemon-launch-helper
3. /usr/kerberos/bin/ksu
4. /usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
5. /usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper
6. /usr/lib/nspluginwrapper/plugin-config
7. /usr/sbin/ccreds_validate
8. /usr/sbin/usernetctl
9. /usr/sbin/suexec
10. /usr/sbin/userhelper
11. /usr/bin/staprun
12. /usr/bin/Xorg
13. /usr/bin/crontab
14. /usr/bin/chage
15. /usr/bin/sudo
16. /usr/bin/sudoedit
17. /usr/bin/passwd
18. /usr/bin/rlogin
19. /usr/bin/at
20. /usr/bin/newgrp
21. /usr/bin/rcp
22. /usr/bin/chsh
23. /usr/bin/gpasswd
24. /usr/bin/chfn
25. /usr/bin/rsh
26. /usr/libexec/openssh/ssh-keysign
27. find: /proc/15008/task/15008/fd/4: No such file or directory
28. find: /proc/15008/fd/4: No such file or directory
29. /sbin/umount.nfs
30. /sbin/umount.nfs4
31. /sbin/mount.nfs
32. /sbin/unix_chkpwd
33. /sbin/mount.ecryptfs_private
34. /sbin/pam_timestamp_check
35. /sbin/mount.nfs4
36. /bin/ping6
37. /bin/umount
38. /bin/ping
39. /bin/mount
40. /bin/su
41. [root@localhost testlinux]#
```

## 3、添加用户、组的命令

### 3.1、添加、删除用户

useradd 设置选项 用户名 -D查看缺省参数 
-u:UID
-g:缺省所属用户组GID
-G:指定用户所属多个组
-d:宿主目录
-s:命令解释器Shell
-c:描述信息
-e:指定用户失效时间
passwd username
手工添加用户 

eg.

```shell
1. [root@localhost testlinux]# useradd -u 6666 -g root -G sys,apache -d /backupzhang -s /bin/bash -c “prject zhangxiaoguang” -e 2018-11-14 jackzhang
```

userdel -r username 
-r删除用户目录 
查找home目录中属于jack的文件有哪些

```
1. [root@localhost ~]# find /home -user jack
2. /home/jack
3. /home/jack/.bash_profile
4. /home/jack/.bashrc
5. /home/jack/.mozilla
6. /home/jack/.mozilla/extensions
7. /home/jack/.mozilla/plugins
8. /home/jack/.bash_logout
9. [root@localhost ~]#
```

### 3.2、添加、删除、修改用户组

添加用户组groupadd 
范例：

```
1.  groupadd -g 888 webadmin
```

创建用户组，其GID为888
删除用户组 groupdel

```
1.  groupdel groupname
```

修改用户组信息groupmod 

groupmod -n apache webadmin 

修改webadmin组名为apache

### 3.3、设置组密码及管理组内成员

gpasswd 
-a添加用户到用户组 group -a username groupname
-d从用户组删除用户
-A设置用户组管理员
-r删除用户组密码
-R禁止用户切换为该组
gpasswd groupname 设置组密码

### 3.4、修改用户信息

usermod
usermod -G groupname username 
eg.

```
1. usermod -G softgroup samlee
```

将用户samlee添加到softgroup用户组中

### 3.5、切换用户组

newgrp group 
切换用户组，如果组有密码需要输入组密码

### 3.6、查看id信息

id 
显示用户id，组id等信息 

```
1. [centos@localhost testlinux]$ id
2. uid=500(centos) gid=500(centos) groups=500(centos) context=root:system_r:unconfined_t:SystemLow-SystemHigh
3. [centos@localhost testlinux]$ 
```

### 3.7、查看隶属组

groups

```
1. [centos@localhost testlinux]$ groups
2. centos
3. [centos@localhost testlinux]$
```

### 3.8、用户组管理命令

groups 查看用户隶属于哪些用户组
newgrp 切换用户组
grpck 用户组配置文件检测
chgrp 修改文件所属组
vigr 编辑/etc/group文件（锁定文件）
groups username 
newgrp groupname 
chgrp groupname

### 3.9、用户管理命令

pwck 检测/etc/passwd文件
vipw 编辑/etc/passwd文件（锁定文件）
id 查看用户id和组信息
finger 查看用户详细信息
su 切换用户(su - 环境变量切换)
passwd -S 查看用户密码状态
who、w 查看当前用户登录信息
pwck命令检测/etc/passwd文件是否有误 
vipw编辑/etc/passwd文件时锁定文件，防止多个用户同时修改 
finger username 查看用户详细信息 
su切换用户
su username 不会切换环境变量
su - username 切换用户同时切换环境变量
锁定用户 passwd -l jack or usermod -L　username 
解锁用户 passwd -uf jack or usermod -U　username

### 3.10、用户密码管理命令chage

chage 设定密码 
-l 查看用户密码设置 
-m 密码修改最小天数 
-M 密码修改最大天数 
-d 密码修改的日期 
-I 密码过期后，锁定的天数 
-E 设置密码的过期日期，如果为0，代表密码立即过期； 
如果为-1，代表密码用不过期 
-W 设置密码过期前，开始警告的天数

### 3.11、启用、停用shadow命令

启用或停用shadow功能 
pwconv / pwunconv 
grpconv / grpunconv
system-config-users 启动图形工具
anthconfig 、/etc/sysconfig/anthconfig 管理工具

### 3.12、范例

授权用户jack和mary对目录/software有写权限

```
1. [root@localhost ~]# useradd jack
2. [root@localhost ~]# useradd mary
3. [root@localhost ~]# mkdir /software
4. [root@localhost ~]# groupadd softadm
5. [root@localhost ~]# usermod -G softadm jack
6. [root@localhost ~]# gpasswd -a mary softadm
7. Adding user mary to group softadm
8. [root@localhost ~]# chgrp softadm /software
9. [root@localhost ~]# chmod g+w /software/
10. [root@localhost ~]# ls -ld /software/
11. drwxrwxr-x 2 root softadm 4096 Oct 14 06:22 /software/
12. [root@localhost ~]#
13. [root@localhost ~]# grep softadm /etc/group
14. softadm:x:505:jack,mary
15. [root@localhost ~]#
```

## 4、批量管理

### 4.1、批量添加用户

newusers命令导入用户信息 
pwunconv命令取消shadow password功能 
chpasswd命令 导入密码文件 
(格式　用户名:密码) 
pwconv命令 将密码写入shadow文件 
示例：一次批量添加10个用户 
在/test目录编写users.info文件

```
1. vi /test/users.info
1. user01::1001:505::/home/user01:/bin/bash
2. user02::1002:505::/home/user02:/bin/bash
3. user03::1003:505::/home/user03:/bin/bash
4. user04::1004:505::/home/user04:/bin/bash
5. user05::1005:505::/home/user05:/bin/bash
```

导入用户

```
1. [root@localhost ~]# newusers < /test/users.info 
2. [root@localhost ~]#
```

这样做的好处是不用手动创建宿主目录。 
取消Shadow password功能

```
1. [root@localhost ~]# pwunconv
2. [root@localhost ~]#
```

生成密码文件

```
1. user01:123456
2. user02:123456
3. user03:123456
4. user04:123456
5. user05:123456
```

导入密码

```
1. [root@localhost ~]# chpasswd < /test/pass.info 
2. [root@localhost ~]#
```

做到这一步就导入了生成了用户和在/etc/passwd文件中导入了用户的密码

```
1. [root@localhost ~]# tail -5 /etc/passwd
2. user01:$1$dOi8eXwo$uosW/Y1xABAgP6KNnHT6h.:1001:505::/home/user01:/bin/bash
3. user02:$1$dOi8eXwo$uosW/Y1xABAgP6KNnHT6h.:1002:505::/home/user02:/bin/bash
4. user03:$1$dOi8eXwo$uosW/Y1xABAgP6KNnHT6h.:1003:505::/home/user03:/bin/bash
5. user04:$1$dOi8eXwo$uosW/Y1xABAgP6KNnHT6h.:1004:505::/home/user04:/bin/bash
6. user05:$1$dOi8eXwo$uosW/Y1xABAgP6KNnHT6h.:1005:505::/home/user05:/bin/bash
```

转换密码

```
1. [root@localhost ~]# pwconv
2. [root@localhost ~]# tail -5 /etc/passwd
3. user01:x:1001:505::/home/user01:/bin/bash
4. user02:x:1002:505::/home/user02:/bin/bash
5. user03:x:1003:505::/home/user03:/bin/bash
6. user04:x:1004:505::/home/user04:/bin/bash
7. user05:x:1005:505::/home/user05:/bin/bash
8. [root@localhost ~]#
```

4.2、限制用户切换root
限制用户切换为root

``` 
#groupadd sugroup 
#chmod 4550 /bin/su 
#chgrp sugroup /bin/su 
#ls -l /bin/su 
-r-sr-x--- 1 root sugroup 18306 Jan 15 2010 /bin/su 
```
设定后，只有sugroup组中的用户可以使用su切换为root 

```
useradd helen 
passwd helen 
usermod -G sugroup helen
```

用sudo代替su
在执行sudo命令时，临时成为root
不会泄漏root口令
仅向用户提供有限的命令使用权限
配置文件:/etc/sudoers,编辑配置文件命令visudo,普通用户使用sudo命令 
格式 
用户名(组名) 主机地址=命令 （绝对路径）
范例：授权用户可以添加用户权限

```
1. centos helen=/usr/sbin/useradd,/usr/sbin/userdel
2. centos helen=/usr/sbin/shutdown -h now #精确化命令，不能用其他选项，只能立即关机
```

### 4.3、破解密码
John the ripper应用
```
#tar -xzvf john-1.7.6.tar.gz 
#cd john-1.7.6 
#make 
```
破解用户密码
```
1. grep liming /etc/passwd > /test/liming.passwd
2. grep liming /etc/shadow > /test/liming.shadow
3. /test/john-1.7.6/run/unshadow /test/liming.passwd /test/liming.shadow > /test/liming.john
4. /test/john-1.7.6/run/john /test/liming.john
```
# 五、进程管理

### 1、进程基础概念
进程和程序的和区别
程序是静态的 
进程，是程序执行的过程，有生命周期 
程序和进程无一一对应关系
**父进程和子进程**
- 子进程是由一个进程所产生的进程，产生这个子进程的进程成为父进程
- 在Linux系统中使用fork创建进程
- fork复制的内容包括父进程的数据和堆栈以及父进程的进程环境。
- 父进终止子进程自然终止
  **台进程和后台进程**
  **前台进程 **
  用户在终端输入一个命令，创建了一个进程，显示提示给用户的信息，在这个进程运行结束之前，用户不可再执行别的命令。
  **后台进程**
  在终端输入一个命令，随后跟一个&，shell创建进程运行这个命令，不等命令退出，而直接返回。后台进程必须是非交互的。
## 2、进程相关的命令
### 2.1、查看用户信息w
w显示用户信息的含义

- JCPU 以终端代号来区分，该终端所有相关进程执行时，所消耗的CPU时间会显示在这里。
- PCPU:CPU执行程序耗费的时间
- WHAT:用户正在执行的操作
- load average :分别显示系统过去1、5、15分钟的平均负载程度
- FROM:显示用户从何处登录系统，“:0”的显示代表该用户时从X Window下，打开文本窗口登录的
- IDEL:用户闲置时间，这是一个计时器，一旦用户执行任何操作，该计时器会被重置。 

查看个别用户信息 w 用户名

```
1. [root@localhost ~]# w
2.  02:33:03 up 2 days, 10:55,  4 users,  load average: 0.06, 0.07, 0.02
3. USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
4. centos   :0       -                06Oct16 ?xdm?   1:16m  0.38s /usr/bin/gnome-session
5. root     pts/0    192.168.9.22     02:00    0.00s  0.31s  0.00s w
6. centos   pts/1    :0.0             Fri22   27:54m  0.02s  0.02s bash
7. [root@localhost ~]#
```
### 2.2、查看系统中的进程ps
常用选项
-a显示所有用户的进程
-u显示用户名和启动时间
-x显示没有控制终端的进程
-e显示所有进程，包括没有控制终端的进程
-l长格式显示
-w宽行显示，可以使用多个w进行加宽显示
ps常用输出信息的含义
- PID :进程号
- PPID:父进程的进程号
- TTY:进程启动的终端
- STAT:进程的当前状态 
- S休眠状态,D不可中断的休眠状态，R运行状态，Z僵死状态，T停止
- NI进程优先级
- TIME:进程自从启动以来启动CPU的总时间
- COMMAND/CMD:进程的命令名
- USER:用户名
- %CPU:占用CPU时间和总时间的百分比
- %MEM:占用内存与系统内存总量的百分比
  ps 应用示例

``` 
ps查看隶属自己的进程
ps -u or -l查看隶属于自己进程详细信息 
ps -le or -aux查看所有用户执行的进程的详细信息 
ps -aux --sort pid可按进程执行的时间、PID、UID等对进程进行排序 
ps -aux | grep sam ps -uU sam查看系统中指定用户执行的进程 
ps -le | grep init查看指定的进程信息 
pstree查看进程树
```
### 2.3、杀死进程kill

**为什么要杀死进程**

- 该进程占用了过多的CPU时间
- 该进程锁住了一个终端，使其他前台进程无法运行
- 运行时间过长，但没有预期效果
- 产生了过多到屏幕或磁盘的输出
- 无法正常退出


**关闭进程的方法**

- 关闭进程 kill 进程号
- 强行关闭kill -9 进程号
- 重启进程kill -1 进程号
- 关闭图形程序xkill
- 结束所有进程killall
- 查找服务进程号pgrep 服务名称
- 关闭进程pkill 进程名称 或 killall 进程名称

### 2.4、优先级命令
nice
- 指定程序的运行优先级
- 格式: nice -n command
- 例如：nice -5 myprogram


renice

- 改变一个正在运行的进程的优先级
- 格式：renice n pid
- 例如：renice -5 777
- 优先级取值范围(-20~19)

### 2.5、用户退出仍然执行nohub
使进程在用户退出登录后仍然继续执行,nohub命令将执行后的数据信息和错误信息默认存储到文件nohub.out中 
格式：nohub program &
### 2.6、进程的挂起和恢复
进程的中止(挂起)和终止
- 挂起(Ctrl＋Z)
- 终止(Ctrl+C)


进程的恢复

- 恢复到前台继续执行(fg)
- 恢复到后台继续执行(bg)
- 查看被挂起的进程(jobs)

### 2.7、动态显示进程top
job作用
进程状态动态显示和进程控制，每5秒钟自动刷新一次（动态显示）
常用选项：
- d:指定刷新的时间间隔
- c:显示整个命令行而不仅仅显示命令名
- u:查看指定用户的进程
- k:终止执行中的进程
- h or ? :获得帮助
- r:重新设置进程优先级
- s:改变刷新的时间间隔
- W:将当前设置写入~/.toprc中

## 3、计划任务
为什么需要计划任务
定时完成某事，周期做某事
计划任务的命令
- at安排作业在某一时刻执行一次
- batch 安排作业在系统负载不重时执行一次
- cron 安排周期性运行的作业

### 3.1、一次性任务at
功能
安排一个或多个命令在指定的时间运行一次 
at的命令格式及参数
at的格式及参数
- at [-f 文件名] 时间
- at -d or atrm 删除队列中的任务
- at -l or atq 查看队列中的任务
- at计划任务保存位置/var/spool/at/

at命令指定时间的方法
**绝对计时方法**

- midnight noon teatime
- hh:mm[today]
- hh:mm tomorrow
- hh:mm 星期
- hh:mm MM/DD/YY

**相对计时法**
- now + n minutes
- now + n hours
- now + n days

范例
指定在今天下午17:30执行某命令(假设现在时间是下午14:30,2011年1月11日)
命令格式如下：

- at 5:30pm
- at 17:30
- at 17:30 today
- at now + 3 hours
- at now + 180 minutes
- at 17:30 11.1.11
- at 17:30 1/11/11


交互方式
at 9:00
范例：5分钟后执行一些命令

```
1. [root@localhost testlinux]# at now +5 minutes
2. at> ls /
3. at> /usr/bin/wall Hello...at...command!<EOT>
4. job 1 at 2016-10-16 05:21
5. [root@localhost testlinux]#
```

输入完at now +5 minutes回车，会进入交互模式 
可以连续多个命令，每个输完按回车 
输完了，按Ctrl+D保存即可
查看计划任务及删除

```
1. [root@localhost testc]# at -l
2. 3   2016-10-16 05:52 a root
3. [root@localhost testc]#at -d 3 
```

使用命令文件方式

1. 生成文件at.script:
2. 使用at命令

```
1. at -f at.script 9:00 2/2/11
2. or
3. at < at.script 9:00 2/2/11
```

at配置文件
作用：限制哪些用户可以使用at命令 

- /etc/at.allow 
- /etc/at.deny
  如果/etc/at.allow文件存在，那么只有列在此文件中的用户才可以使用at命令，若/etc/at.allow文件不存在，则检查/etc/at.deny文件是否存在。若/etc/at.deny存在，则在此文件中列出的用户都不能使用at命令。 
  如果两个文件都不存在，则只有超级管理员可以使用at命令。 
  如果两个文件都存在而且均为空，则所有用户都可以使用at命令。

### 3.2、一次性batch命令
作用：
安排一个或多个命令在系统负载较轻时运行一次，（一般情况下负载较轻指平均负载降到0.8以下）
使用方法同at
### 3.3、周期计划任务crontab
作用：
用于生成cron进程所需要的crontab文件
crontab的命令格式
crontab{-l|-r|-e}

- -l 显示当前的crontab
- -r 删除当前的crontab
- -e 使用编辑器编辑当前的crontab文件

**crontab文件格式**
minute hour day-of-month month-of-year day-of-week commands
- minutes 一小时中的哪一分钟[0~59]
- hour一天中的哪个小时[0~23]
- day-of-month一个月中的哪一天[1~31]
- month-of-year一年中的哪个月[1~12]
- day-of-week 一周中的哪一天[0~6]
- commands 执行命令


**书写的注意事项**

- 选项都不能为空，必须填入，不知道的统统用*表示任何时间
- 每个时间字段都可以指定多个值
- 不连续的用逗号，分割
- 连续的值用-分割
- 每/[分钟，小时]用：*/n，如分钟字段填*/2每隔两分钟
- 命令应该给出绝对路径
- 用户必须有对应运行对应的命令或程序的权限

范例：

```
1. 分钟  小时  天   月   星期  命令/脚本
2. 0       4       *   *       *       //每天4：00
3. 0       18      *   *       2,5     //周二和周五的18:00
4. 0       18      *   1-3     2,5     //一月到三月的星期二和星期五的18:00
5. 30      17      *   *       2,5     //周二和周五的17:30
6. 45      17      *   *       2,5     //周二和周五的17:45
7. */2     12-14   * 3-6,9-12 1-5  //3月到6月和9月到12月的周一到周五的12:00到14:00每个两分钟
```

计划任务保存文件目录/var/spool/cron/root,编辑好计划任务，可以查看这个文件
编辑用crontab -e
也可以直接编辑/var/spool/cron/root
crontab的配置文件
作用：限制哪些用户可以使用crontab

- /etc/cron.allow
- /etc/cron.deny
- /etc/crontab

查看/etc/crontab

```
1. [root@localhost ~]# cat /etc/crontab
2. SHELL=/bin/bash
3. PATH=/sbin:/bin:/usr/sbin:/usr/bin
4. MAILTO=root
5. HOME=/
6. 
7. # run-parts
8. 01 * * * * root run-parts /etc/cron.hourly
9. 02 4 * * * root run-parts /etc/cron.daily
10. 22 4 * * 0 root run-parts /etc/cron.weekly
11. 42 4 1 * * root run-parts /etc/cron.monthly
```

这些事系统定义的计划任务 
可以修改，也可以把我们的也加进去

## 4、进程的处理方式
处理方式有如下几种
- standalone独立运行
- xineted进程托管
- atd、crond计划任务

standalone独立运行模式
standalone模式就是一直监听运行的程序，例如apache的httpd就是这种进程。这种进程特点是耗费系统资源很多，但是效应速度很快。因为一直在监听
范例:查看监听的接口 
netstat -an | grep "LISTEN" | more

```
1. [root@localhost ~]# netstat -an | grep "LISTEN" | more
2. tcp        0      0 0.0.0.0:934                 0.0.0.0:*                   LISTEN      
3. tcp        0      0 0.0.0.0:111                 0.0.0.0:*                   LISTEN      
4. tcp        0      0 127.0.0.1:631               0.0.0.0:*                   LISTEN      
5. tcp        0      0 127.0.0.1:25                0.0.0.0:*                   LISTEN      
6. tcp        0      0 :::80                       :::*                        LISTEN      
7. tcp        0      0 :::22                       :::*       				   LISTEN
```

xineted进程托管
如ftp服务，管理目录/etc/xinetd.d，telnet等 
对应进程 
ps -le | grep xinetd
进程托管，就是通过xinetd来进行监听端口，当有客户端访问，xinetd再交给对应的程序去执行。
范例，查看telnet的配置

```
1. [root@localhost xinetd.d]# cat krb5-telnet 
2. # default: off
3. # description: The kerberized telnet server accepts normal telnet sessions, \
4. #              but can also use Kerberos 5 authentication.
5. service telnet
6. {
7.     flags       = REUSE
8.     socket_type = stream        
9.     wait        = no
10.     user        = root
11.     server      = /usr/kerberos/sbin/telnetd
12.     log_on_failure  += USERID
13.     disable     = yes # 是否启用
14. }
```

atd、crond计划任务
每一分钟检查一次列表形式，所以不能精确到秒



# 六、文件系统管理
大纲
- 文件系统构成
- 设备挂载
- 分区和格式化原理
- 磁盘配额
## 1、文件系统构成

- /usr/bin、/bin ： 存放所有用户可以执行的命令
- /usr/sbin、/sbin ：存放只有root才可以执行的命令
- /home ：用户缺省的宿主目录
- /proc ：虚拟文件系统，存放当前的内存镜像
- /dev :存放设备文件 
  所有设备的文件
- /lib:存放所有程序运行所需要的共享库
- /lose+found : 存放一些系统出错的检查结果 
  系统出错时可以看一下
- /tmp ：存放用户临时文件
- /etc ：系统配置文件，很重要建议备份
- /var ：包括经常发生变动的文件，如邮件、日志文件、计划任务等
- /usr ：存放所有命令、库、手册等
- /mnt ：临时文件系统的安装点
- /boot ：内核文件及自举程序文件保存位置

## 2、常用命令
- 查看分区使用情况：df
- 查看文件、目录大小：du
- 检测修复文件系统：fsck、e2fsck（单用户模式执行）
- 判断文件类型：file


查看分区 df

```
1. [root@localhost /]# df
2. Filesystem           1K-blocks      Used Available Use% Mounted on
3. /dev/sda2             78928680   2368852  72485744   4% /
4. /dev/sda1               295561     16200    264101   6% /boot
5. tmpfs                   517552         0    517552   0% /dev/shm
6. [root@localhost /]#
```

df -h人性化显示

```
1. [root@localhost /]# df -h
2. Filesystem            Size  Used Avail Use% Mounted on
3. /dev/sda2              76G  2.3G   70G   4% /
4. /dev/sda1             289M   16M  258M   6% /boot
5. tmpfs                 506M     0  506M   0% /dev/shm
6. [root@localhost /]#
```

解读
根分区/ 大小76G 
/boot分区 289M 
tmpfs 临时文件系统分区 506M
sda1、sda2：sd是硬盘，a是第一个，1和2是第一个分区，第二个分区 
所以就是： 
sda1:第一块硬盘的第一个分区 
sda2:第一块硬盘的第二个分区
df -m 以M单位显示

```
1. [root@localhost /]# df -m
2. Filesystem           1M-blocks      Used Available Use% Mounted on
3. /dev/sda2                77079      2314     70787   4% /
4. /dev/sda1                  289        16       258   6% /boot
5. tmpfs                      506         0       506   0% /dev/shm
6. [root@localhost /]#
```

查看文件夹和文件大小du
文件大小命令du - h filename 
范例：

```
1. [root@localhost /]# du -h /etc/services 
2. 364K    /etc/services
3. [root@localhost /]#
```

目录大小命令：du -sh /etc 
范例：

```
1. [root@localhost /]# du -sh /etc
2. 157M    /etc
3. [root@localhost /]#
```

修复文件系统
当出现异常断电或其他原因导致文件系统损坏，可进入单用户模式，使用fsck命令进行修复 
fsck -p 名称或fsck -y 名称
查看文件类型

```
1. [root@localhost /]# file /etc/services 
2. /etc/services: ASCII English text
3. [root@localhost /]#
```

## 3、使用光驱
**挂载光驱**
```
1. mkdir /mnt/cdrom
2. mount /dev/cdrom /mnt/cdrom
3. df
4. cd /mnt/cdrom
5. ls /mnt/cdrom
```
**卸载光驱**
```
1. umount /mnt/cdrom
2. eject
```
测试：在vmware挂载dvd 
设置Vmware: 
虚拟机-->设置-->cd/DVD-->连接：使用ISO镜像 
设备状态：**选已连接（重要）**
在centos中：
```
1. [root@localhost /]# mkdir /mnt/cdrom
2. [root@localhost /]# mount /dev/dvd-hda /mnt/cdrom/
3. mount: block device /dev/dvd-hda is write-protected, mounting read-only
4. [root@localhost /]# cd /mnt/cdrom/
5. [root@localhost cdrom]# ls
6. manifest.txt  vmware-solaris-tools.tar.gz
```
查看挂载系统
```
1. [root@localhost cdrom]# df -h
2. Filesystem            Size  Used Avail Use% Mounted on
3. /dev/sda2              76G  2.3G   70G   4% /
4. /dev/sda1             289M   16M  258M   6% /boot
5. tmpfs                 506M     0  506M   0% /dev/shm
6. /dev/hda               14M   14M     0 100% /mnt/cdrom
```
成功挂载！
卸载：
```
1. [root@localhost cdrom]# cd ..
2. [root@localhost mnt]# umount /mnt/cdrom/
3. [root@localhost mnt]
```
注意卸载的时候，当前目录不能是挂载后的目录
或直接使用eject直接卸载
mount: block device：在linux系统中文件分为块设备，如U盘，硬盘，光盘，这种大数据读存取的叫做块设备，文件类型代号b, 
还有charset device:就是字节设备，如打印机。。。代号c

## 4、添加磁盘或分区

0



# 七、Shell 脚本编程
## 1、简单的shell语法
### 1.1、shell的结构
shell的结构

!指定执行脚本的shell

注释行

命令和控制结构 

创建shell程序的步骤 
第一步：创建一个包含控制命令和控制结构的文件 
第二步：修改这个文件的权限使他可以执行，chmod u+x 
第三步：执行./example 
(也可以使用sh example执行) 
eg.example.sh

```
1. #!/bin/bash
2. # This is to show that a example looks like.
3. echo "Our first example"
4. echo # This is an empty line in output.
5. echo "We are currently in the following directory."
6. /bin/pwd
7. echo
8. echo "This directory contains the following files."
9. /bin/ls
```

eg.查看系统信息脚本 
周一到周五发消息

```
1. #!/bin/sh
2. # auto mail for system info
3. 
4. /bin/date +%F >> /tmp/sysinfo
5. 
6. echo "disk info:" >> /tmp/sysinfo
7. /bin/df -h >> /tmp/sysinfo
8. 
9. echo >> /tmp/sysinfo
10. echo "online users:" >> /tmp /sysinfo
11. /usr/bin/who | /bin/grep -v root >> /tmp/sysinfo
12. 
13. echo >> /tmp/sysinfo
14. echo "memory info:" >> /tmp/sysinfo
15. /usr/binfree -m >> /tmp/sysinfo
16. echo >> /tmp/sysinfo
17. 
18. #write root
19. /usr/bin/write root < /tmp/sysinfo &
20. 
21. # crontab -e
22. # 0 9 * * 1-5 script
```

### 1.2、Shell变量
变量：是shell传递数据的一种方法，用来代表每个取值的符号名。 
Shell有两类变量：临时变量，永久变量。
变量定义用字母数字下划线 
引用变量$ 
范例：
```
1. NUM=1
2. A=$B
3. echo $A
```
多个字时用引号括起来 
echo "Mike Ron" 
单引号和双引号区别
```
1. DATE=$(date +%F)
2. ABC="time is $DATE"
3. echo $ABC
4. 2016-10-15
5. ABC='time is $DATE'
6. echo $ABC
7. time is $DATE
```
set 命令查看所有变量 
unset command 删除变量

位置变量 
Shell解释执行用户命令时，将命令行的第一部分作为命令名，其他部分作为参数。由出现在命令行上的位置确定的参数成为位置参数 
例如：

1. ls -l file1 file2 file3
   $0这个程序的文件名ls -l 
   $n这个程序的第n个参数n = 1~9 
   $*这个程序的所有参数 
   $#这个程序的参数个数 
   $$这个程序的PID 
   $!执行上一个后台命令的PID 
   $?执行上一个命令的返回值


范例：自动备份脚本 
autobak.sh

```
1. #!/bin/bash
2. #backup files by date
3. BACK_DIR=/backup
4. 
5. # Is /backup directory exists?
6. if [ -d $BACK_DIR ]
7. then
8. 	echo "$BACK_DIR exists!"
9. else
10.     echo "$BACK_DIR isn't exists!"
11. 	mkdir $BACK_DIR
12.     if [ $? -eq 0 ]
13. 	then
14. 		echo "Create backup's directory $BACK_DIR Successfully!"
15.     fi
16. fi
17. 
18. DATE=`/bin/date +%Y%m%d`
19. /bin/tar -cf /backup/$1.$DATE.tar $1 > /dev/null 2>> /backup/$1.bak.log
20. /bin/gzip  /backup/$1.$DATE.tar
21. if [ $? -eq 0 ]
22. then
23.     echo "$1 $DATE backup sucessfully" >> /backup/$1.bak.log
24. else
25.     echo "ERROR: failure $1 $DATE backup !" >> /back/$1.bak.log
26. fi
27. 
28. # crontab -e
29. # 0 3 * * 2,5 script
```

这个脚本有一个bug，不能备份二级目录
范例：测试特殊的变量输出

```
1. #/bin/bash
2. # Test special variable
3. # Usage: sh special.sh file01 file02 ...
4. 
5. echo '$# is:' $#
6. echo '$* is:' $*
7. echo '$? is:' $?
8. echo '$$ is:' $$
9. echo '$0 is:' $0
10. echo '$2 is:' $2
```

### 1.3、读取输入数据命令

read命令：从键盘读入数据，赋给变量 
如：read USERNAME 
read命令让操作者和脚本之间有了互动的关系。 
范例：

```
1. #!/bin/bash
2. # This is test read command's script
3. 
4. echo "Please write 3 parameters"
5. read first second third
6. echo "The first parameter is:$first"
7. echo "The second parameter is:$second"
8. echo "The third parameter is:$third"
```

这个脚本一下子读了3个参数，通常情况下一次只读一个参数 
如果上面的例子，输入了4个参数，那么参数3third就会是参数3和参数4 
如输入：100 200 300 400 
输出：

```
1. [root@localhost testshell]# sh readcmd.sh 
2. Please write 3 parameters
3. 100 200 300 400
4. The first parameter is:100
5. The second parameter is:200
6. The third parameter is:300 400
```

### 1.4、脚本的调试
在执行脚本的时候，向看下脚本执行到哪一步，用 
sh -x script 
-x 参数来打印 
范例：

```
1. [root@localhost testshell]# sh -x readcmd.sh 
2. + echo 'Please write 3 parameters'
3. Please write 3 parameters
4. + read first second third
5. 100 200 300         
6. + echo 'The first parameter is:100'
7. The first parameter is:100
8. + echo 'The second parameter is:200'
9. The second parameter is:200
10. + echo 'The third parameter is:300'
11. The third parameter is:300
12. [root@localhost testshell]#
```

这样方便调试

### 1.5、算数命令expr
shell脚本通常还需要进行一些数学运算来完成我们需要的结果。 
shell变量的算数运算 
expr命令：对整数型变量进行数学运算 
例如：
``` 
expr 3 + 5 
expr $var - 5 
expr $var1 / $var2 
expr $var1 \* $var2 
expr 10 % 3 
```
注意变量与运算符之间要隔一个空格。
复杂的expr运算

```
1. expr `expr 5 + 7`/var4
```

将运算结果赋予变量

```
1. var4=`expr $var1/$var2`
```

### 1.6、使用多个命令

多个命令串起来使用顺序执行
多个命令顺序执行在一行输入，用;隔开

```
root@ubuntu# date;who
Mon Sep 26 161015 PDT 2016
ubuntu   tty7         2016-09-26 1507 (0)
root@ubuntu#
```

这里注意最大的命令行字数是255。

## 2、变量测试语句
变量测试语句：用于测试变量是否相等、是否为空，文件类型等 
格式： 
test 测试条件 
测试范围：整数、字符串、文件
字符串测试
```
test str1=str2 测试字符串是否相等 
test str1!=str2测试字符串是否不相等 
test str测试字符串是否不为空 
test -n str测试字符串是否不为空 
test -z str测试字符串是否为空
```
变量测试语句
```
test int1 -eq int2测试整数是否相等 
test int1 -ge int2测试int1是否>=int2 
test int1 -gt int2测试int1是否>int2 
test int1 -le int2测试int1是否<=int2 
test int1 -lt int2测试int1是否< int2 
test int1 -ne int2测试整数是否不相等
```
文件测试
```
test -d file指定的文件是否为目录 
test -f file指定的文件是否为常规文件 
test -x file指定的文件是否可执行 
test -r file指定的文件是否可读 
test -w file指定的文件是否可写 
test -a file指定的文件是否存在 
test -s file文件的大小是否非0
```
变量测试语句一般不单独使用，一般做if语句的测试条件如：

```
1. if test -d $1 then
2.  ...
```

变量测试语句可用[]进行简化，如： 
`test -d $1等价于[ -d $1 ]`
范例：测试apache服务是否启动了，如果没有启动，那么将其启动 
apachetest.sh

```
1. #!/bin/sh
2. # "if...else" usage
3. # Using this program to show your system'sservices
4. 
5. echo "Now,this web services of ths Linux system will be detect..."
6. echo
7. 
8. # Detect www service
9. web=`/usr/bin/pgrep httpd`
10. if [ "$web" != "" ]
11. then
12.     echo "The web service is running."
13. else
14.     echo "The web services is NOT running."
15.     /etc/rc.d/init.d/httpd start
16. fi
```

pgrep可以判断启动程序的PID，当程序没有执行，pgrep就会返回空 
范例： 
pgrep httpd 
pkill可以关闭服务进程 
范例： 
pkill httpd

## 3、逻辑控制语句
流程控制语句：用于控制shell程序的流程
### 3.1、程序退出
exit语句：退出程序执行，并返回一个返回码，返回0表示正常退出，返回非0表示非正常推出。 
例如：exit 0
### 3.2、逻辑控制if语句
if...then...fi语句
例如：

```
1. #!/bin/bash
2. if [ -x /etc/rc.d/init.d/httpd ]
3. then
4.     /etc/rc.d/init.d/httpd start
5. fi
```
if/else 语句
```
1. if xxxx
2. then 
3.     ...
4. else
5.     ...
6. fi
```
复杂的if语句
```
1. if 条件1 then
2.     commands
3. elif 条件2 then
4.     commands
5. else
6.     commands
7. fi
```
条件判断支持嵌套使用。
示例：判断文件类型 
filetype.sh
```
1. #!/bin/bash
2. # This script is output filetype's script.
3. 
4. echo "Please input a file name:"
5. read FILE_NAME
6. if [ -d $FILE_NAME ]
7. then
8. 	echo "$FILE_NAME is a directory."
9. elif [ -f $FILE_NAME ]
10. then
11. 	echo "$FILE_NAME is a common file"
12. elif [ -c $FILE_NAME -o -b $FILE_NAME ]
13. then
14.     echo "$FILE_NAME is a device file"
15. else
16. 	echo "$FILE_NAME is an unknow file"
17. fi
```
示例：比较两个数大小的脚本 
numbermax.sh
```
1. #/bin/bash
2. if [ $# -ne 2 ];then
3. 	echo "Not enough parameters"
4. 	exit 0
5. fi
6. if [ $1 -eq $2 ];then
7. 	echo "$1 equals $2"
8. elif [ $1 -lt $2 ];then
9. 	echo "$1 littler than $2"
10. elif [ $1 -gt $2 ];then
11. 	echo "$1 greater than $2"
12. fi
```
多个条件联合
-a逻辑与，仅当两个条件都成立时，结果为真 
-o逻辑或，两个条件只有一个成立时，结果为真

### 3.3、循环语句
for循环
for...done语句 
格式：
```
1. for 变量 in 名字表
2. do
3.     commands
4. done
```
范例：输出星期的脚本 
weekoutput.sh
```
1. #!/bin/bash
2. # Output week day's script
3. 
4. for DAY in Sunday Monday Tuesday Wednesday Thursday Friday Saturday
5. do
6.     echo "The day is : $DAY"
7. done
```
范例：将用户踢出登录的脚本 
killuser.sh
```
1. #!/bin/bash
2. # The script to kill logined user
3. 
4. USER_NAME="$1"
5. 
6. /bin/ps aux | /bin/grep $USER_NAME | /bin/awk '{print $2}' > /tmp/temp.pid
7. 
8. KILL_ID=`cat /tmp/temp.pid`
9. 
10. for PID in $KILL_ID
11. do
12. 	/bin/kill -9 $PID 2> /dev/null
13. done
```
### 3.4、awk命令
awk是在输入信息中提取内容的命令
格式：
``` 
awk [-F 域分隔符] ‘命令’ 
```
如果不指定分隔符那么就是空格是分割符 
如 
```
part1:part2:part3:part4 
```
那么$1代表part1,$2是part2,$n是partn
示例：
``` shell
#1、检测系统中UID为0的用户 
awk -F:'$3==0{print $1}'/etc/passwd 
#2、检测系统中密码为空的用户 
awk -F:'length($2)==0{print $1}'/etc/shadow
```
范例：输出用户信息 
userinfo.sh
```
1. #!/bin/bash
2. # Display user's infomation's script
3. 
4. # test user exists?
5. /bin/echo "Please input the username:"
6. read USER_NAME
7. /bin/grep $USER_NAME /etc/passwd > /dev/null 2> /dev/null
8. if [ $? -eq 0 ]
9. then
10.     /bin/echo "user name is: $USER_NAME"
11. else
12. 	/bin/echo "user $USER_NAME does not exist"
13.     exit 1
14. fi
15. /bin/echo
16. 
17. # list /etc/passwd info
18. USER_INFO=`/bin/grep ^$USER_NAME:x /etc/passwd`
19. USER_ID=`/bin/echo $USER_INFO | /bin/awk -F: '{print $3}'`
20. GROUP_ID=`/bin/echo $USER_INFO | /bin/awk -F: '{print $4}'`
21. HOME_DIR=`/bin/echo $USER_INFO | /bin/awk -F: '{print $6}'`
22. SHELL=`/bin/echo $USER_INFO | /bin/awk -F: '{print $7}'`
23. 
24. # get group name from GID
25. GROUP_NAME_TMP=`cat /etc/group | /bin/grep :x:$GROUP_ID`
26. GROUP_NAME=`/bin/echo $GROUP_NAME_TMP | /bin/awk -F: '{print $1}'`
27. 
28. # Output info
29. /bin/echo "user id is: $USER_ID"
30. /bin/echo "default group is : $GROUP_NAME"
31. /bin/echo "home directory is : $HOME_DIR"
32. /bin/echo "shell is : $SHELL"
33. /bin/echo "group members info:"
34. 
35. # Get group members
36. GROUP_MEM=`/usr/bin/groups $USER_NAME`
37. /bin/echo $?
38. /bin/echo $GROUP_MEM
39. /bin/echo
40. 
41. # Get login info
42. USER_LOGIN=`/usr/bin/who | /bin/grep $USER_NAME`
43. if [ "$USER_LOGIN" != "" ]
44. then
45. 	 /bin/echo "$USER_NAME is online"
46. else
47.     /bin/echo "$USER_NAME NOT logged in"
48. fi
```
这里要注意的是
```
USER_INFO=/bin/grep ^$USER_NAME:x /etc/passwd
```
这一行，其中^$USRE_NAME:x是为了精确匹配grep命令，不然有可能会输出多行 
^以$USER_NAME开头



# 八、软件包管理

Linux软件的软件分为两种：二进制包RPM，源代码包：tar.gz

二进制包是开发完，进行打包后的包，定制性差。

## 1、准备工作

准备需要的软件包

![](http://note.youdao.com/yws/public/resource/b1e6fe961e16dc8be1c632dd7e2717b1/xmlnote/WEBRESOURCE34878e9b93bcc163d5eecfee1d5eed0b/23521)

需要的工具：

```shell
gcc gcc-c++ make
```

查看make工具是否安装

```shell
[root@localhost /]# rpm -q make
make-3.81-3.el5
```

检测不到编译工具的错误

![](http://note.youdao.com/yws/public/resource/b1e6fe961e16dc8be1c632dd7e2717b1/xmlnote/WEBRESOURCEc00352328cc3f3da50622374ecc65a62/23540)

检测是否安装了gcc和gcc-c++工具

```shell
[root@localhost /]# rpm -q gcc
gcc-4.1.2-54.el5
[root@localhost /]# rpm -q gcc-c++
gcc-c++-4.1.2-54.el5
[root@localhost /]#
```

如果没有安装，那么就先安装编译的工具。

rpm,yum,两种方式安装，最好使用yum安装

光盘安装方式

创建挂在点

```shell
mkdir /mnt/cdrom
```

挂在光盘

```shell
mount /dev/cdrom/mnt/cdrom
```

查找光盘中的文件

```shell
rpm –i /mnt/cdrom/CentOS/gcc
```

## 2、源代码码包的安装

写一个脚本，解压本目录所有的tar.gz包

```shell
cd /dir
ls *.tar.gz > ls.list
for TAR in `cat ls.list`
do
        tar -zxf $TAR
done
```

源代码包编译步骤：

```shell
1、 解压解包  tar.gz   tar -zxf
2、 ./configure 配置—>生成配置文件
3、 make编译
4、 make install 拷贝，权限
```

# 九、Linux网络设置

## 1、网络参考模型

![](http://note.youdao.com/yws/public/resource/b1e6fe961e16dc8be1c632dd7e2717b1/xmlnote/WEBRESOURCE19a29aefa0ad77a6b97152116dbfc339/23543)

TCP/IP 网卡：
OSI物理层：网卡
数据链路层：MAC ADDR 
网络层：IP、APR、ICMP
传输层：TCP、UDP
TCP/IP应用层：SSH、APACHE
OSI会话层：客户端---------服务器建立连结
表示层：加密、压缩
应用层：服务应用

网卡上的MAC地址：数据链路层
00:0C:29:1D:62:A2
前24byte代表厂商：00:0C:29
后24byte产品标识：1D:62:A2
ifconfig	

```shell
[root@localhost ~]# ifconfig
eth0      Link encap:Ethernet  HWaddr 00:0C:29:1D:62:A2  
          inet addr:192.168.9.21  Bcast:192.168.9.255  Mask:255.255.255.0
          inet6 addr: fe80::20c:29ff:fe1d:62a2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:68 errors:0 dropped:0 overruns:0 frame:0
          TX packets:97 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:7766 (7.5 KiB)  TX bytes:15997 (15.6 KiB)
          Interrupt:67 Base address:0x2024
```

IP地址：网络层

192.168.14.127

IPv6：128byte

TCP/ UDP

TCP:可靠，传输控制协议（断点重发、定时器）全双工/单工/半双工

三次握手

A				B

-------------------------->SYN:请求连接

<-------------------------ACK/SYN:应答请求、等待连接

-------------------------->ACK:建立连接

UDP:传输速度快

## 2、网络访问

FQHN:完整的计算机名

www.baidu.com.

**最后的点：根域**

**com/cn/net/org:****顶级域**

**baidu:****二级域名**

**www****：主机名**

**全世界有****13组根域服务器**

![](http://note.youdao.com/yws/public/resource/b1e6fe961e16dc8be1c632dd7e2717b1/xmlnote/WEBRESOURCEa0a5ee3b67405b4fb83ef218148e82c9/23546)

![](http://note.youdao.com/yws/public/resource/b1e6fe961e16dc8be1c632dd7e2717b1/xmlnote/WEBRESOURCEf2b0a35319840889d191d449ee212326/23549)

访问网络需要知道对方的主机地址和MAC地址，我们一般只知道对方的IP而不知道MAC，那么就需要查询。
查询的协议就是ARP地址解析协议。

![img](http://note.youdao.com/yws/public/resource/86a4da202127376e92d52776c591a605/xmlnote/WEBRESOURCEd97bf1a78367a157ad83171cd38bac5c/23554)

arp广播包

arp的命令：

查看arp缓存表：

```shell
arp –a
```

删除arp记录

```shell
arp –d IP地址
```

手动添加静态记录	

```shell
arp –s IP地址 MAC地址
```

RARP 反向地址解析协议
知道自己的MAC地址，去询问自己IP地址

## 3、域名解析、

Linux:

/etc/hosts

DNS:DomainName System

![](http://note.youdao.com/yws/public/resource/86a4da202127376e92d52776c591a605/xmlnote/WEBRESOURCE5639756ad35c1e819d5bf6ccc276dd7e/23558)

一个网卡可以绑定多个IP地址

添加虚拟网卡：

```shell
ifconfig eth0:1 192.168.77.1
```

## 4、网络配置文件



![img](http://note.youdao.com/yws/public/resource/86a4da202127376e92d52776c591a605/xmlnote/WEBRESOURCEd8371f84e787bd1b83a3a23253f214eb/23561)

修改IP地址

简单修改

```shell
ifconfig eth0 192.168.xxx.xxx
```

修改配置文件

```shell
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

IPADDR=xxx.xxx.xxx.xxx

GATWAY=xxx.xxx.xxx.xxx修改网管

NETMASK=xxx.xxx.xxx.xxx子网掩码

BROADCAST=xxx.xxx.xxx.xxx广播地址

更改主机名

```shell
/etc/sysconfig/network
```

HOSTNAME=主机名

网络启动脚本

```
/etc/rc.d/init.d/network start/stop/restart
```

主机名数据库

```
/etc/hosts
```

网路服务信息：

```shell
/etc/services
```

指定DNS服务器地址

```
/etc/resolv.conf
```

nameserverDNS服务器IP(小于等于3个)空格隔开

扫描主机端口：

```
nmap IP地址
```

## 5、网络管理命令

查看网络端口设置ifconfig

使用/不使用网卡

```shell
ifconfig eth0 down/up
```

检测网络连接：

```
ethtool eth0
```

可以判断网卡有没有插网线



ping 探测远程主机

```
ping –c 次数 –s ping包大小 xxxxxxxx
```

操作路由表route

添加网关：

```shell
route add default gw xxx.xxx.xxx.xxx
```

Zebra路由软件

查看路由路径traceroute

```shell
traceroute www.baidu.com
```

监控网络状态netstat

```
netstat -an
```





# 十、网络共享服务

## 1、FTP

![](http://note.youdao.com/yws/public/resource/5d0c3b48c9baad78d7560a0a395daa48/xmlnote/WEBRESOURCEea211704e436628cfdc1d14af06df4c7/23566)

### 1.1、vsftp的使用

#### 1.1.1、匿名访问

匿名用户名：ftp或anonymous

```
ftp伪用户，宿主目录，就是用户可以访问的目录/var/ftp
```

邮箱密码

配置文件/etc/vsftpd/vsftpd.conf

是否开启动匿名ftp

```
anonymous_enable=YES
```

支持上传：#anon_upload_enable=YES
日志：xferlog_enable=YES
日志存放目录：#xferlog_file=/var/log/xferlog

![](http://note.youdao.com/yws/public/resource/5d0c3b48c9baad78d7560a0a395daa48/xmlnote/WEBRESOURCEdd28765b970ebadf99e5035589bb4d54/23580)

ftp的命令：

连接ftp

```
ftp xxx.xxx.xxx.xxx
```

ls cd 

二进制传输

```shell
bin
```

切换下载目录（本地）

```
lcd
```

下载

```
get
```

下载多个

```
mget
```

上传put

上传多个mput

关闭交互模式	prompt

退出bye

连接ftp:open

输入用户名密码:user

#### 1.1.2、自动化下载

首先写一个下载命令列表
auto.ftp

```shell
open 192.168.9.21
user ftp tpxsky@163.com
bin
prompt
lcd /ftp.bak
mget *
bye
```

执行自动下载

```shell
ftp –n < auto.ftp
```



## 2、scp与rsync设置

### 2.1、ssh

ssh不只是用于远程登录用的一个服务
ssh三个作用：
1. ssh远程登录secureCRT,putty
2. sftp 文件共享（类ftp） 
   SSH Secure File Trans
3. scp 文件共享(类似cp拷贝)

ssh远程登录
ssh 不允许空密码登录 
不要用root登录
格式：
```
ssh 用户名@远程主机ip地址 
```
常用选项：
-2：强制使用第二代ssh协议 
-p:端口，默认22，可以不写
示例：
``` 
ssh -2 sam@192.168.9,21 
```
配置文件：
```
/etc/ssh/sshd_config 
```
Linux 工具:OpenSSH 
Windows平台SSH工具:SSH Workstation
配置文件
``` 
PermitRootLogin yes 限制root用户登录的 
Port设置端口号
```
### 2.2、scp
scp应用
从本地拷贝文件到远程主机
``` 
scp 本地文件 用户名@IP:远程主机目标目录 
scp -r 本地目录 用户名@IP:远程目录
```
从远程主机拷贝文件到本地 
```
scp 用户名@IP:远程目录/文件 本地目录 
scp 用户名@IP:远程目录 本地目录
```
常用选项：
-p保持原有文件属性
-r复制目录
-P指定端口号
对称密钥加密
加密和解密使用同一密钥
优势：速度快
缺点：密钥本身需要交换

**非对称加密**
也称公开密钥加密，使用时生成两个密钥，一个公开存放成为公钥，一个私人持有，成为私钥。
用户用一个密钥加密的数据，用另一个密钥才能解密
优势：安全性好 
缺点：速度慢
所以，加密信息时，通常是对称密钥加密，与非对称加密结合使用。
加密文件：公钥加密，私钥解密 
数字签名：私钥加密，公钥加密

**SSH建立信任主机**
```
主机一 
建立密钥对 
ssh-kenygen -t rsa 
生成公钥：id_rsa.pub
主机二 
获得主机一公钥，并生成认证密钥
1. cat id_rsa.pub >> .ssh/authorized_keys
2. chmod 600 .ssh/authorized_keys
3. chmod 700 .ssh
此时从主机一访问主机二，便不需要密码了
```
### 2.3、增量备份rsync
方便的实现增量备份
可以方便保存整个目录树和文件系统
保持文件的权限、时间、软硬链接等
文件传输效率高
可以使用ssh加密通道
**使用方法**
启用rsync 
编辑配置文件/etc/xinetd.d/rsync 
设置disable=no 
重启xinetd进程，service xinetd restart





# QAQ、

## 1、缺少*libgcc*_*s*.*so*.1库问题解决

```shell
sudo apt-get install gcc-multilib
```



## 2、libstdc++.so.6库安装

```shell
$sudo apt-get install libstdc++6 
$sudo apt-get install lib32stdc++6
```



## 3、securecrt假死 ctrl+s ctrl+q

## 4、MD5值

```shell
echo -n 'admin888' | md5sum
```

## 5、SSH服务

```shell
检查ssh服务是否启动： 
ps -e |grep ssh
\==================
ubuntu默认并没有安装ssh服务，如果通过ssh链接ubuntu，需要自己手动安装ssh-server。判断是否安装ssh服务，可以通过如下命令进行：
$ ssh localhost
ssh: connect to host localhost port 22: Connection refused
如上所示，表示没有还没有安装，可以通过apt安装，命令如下：
$ sudo apt-get install openssh-server
系统将自动进行安装，安装完成以后，先启动服务：
$ sudo /etc/init.d/ssh start
启动后，可以通过如下命令查看服务是否正确启动
$ ps -e|grep ssh
6212 ? 00:00:00 sshd
如上表示启动ok。注意，ssh默认的端口是22，可以更改端口，更改后先stop，
然后start就可以了。改配置在/etc/ssh/sshd_config下，如下所示。
1. $ vi /etc/ssh/sshd_config   
2. 
3. #Package generated configuration file   
4. 
5. # See the sshd(8) manpage for details   
6. 
7. # What ports, IPs and protocols we listen for  
8. 
9. Port 22  
最后，应该是连接的时候了。请看如下命令：
$ ssh exceljava@192.168.158.129
启动、停止和重启SSH:
sudo /etc/init.d/ssh start
sudo /etc/init.d/ssh stop
sudo /etc/init.d/ssh restart
或者
sudo start ssh
sudo stop ssh
sudo restart ssh
卸载SSH
先停掉SSH服务：sudo stop ssh
然后：apt-get –purge remove openssh-server
================================================================
“root@localhost's password:”说明ssh互信没有建立起来
是Jobtracker没有执行起来，我用0.20.203的版本也是这样。一样的提示，找 不到server类。namenode和datanode都能正常运行，但是jobtracker启不来。怀疑那个版本的jar包里面有问题，可能需要重 新编译才行。如果是别的原因，希望高手给予解答。
现在还一直使用0.20.2版本的，没有问题。新出的0.20.204和0.20.205版本都没有试过。
另外： 
“root@localhost's password:”说明ssh互信没有建立起来。 
执行 
ssh-keygen -t dsa出现提示都回车就行。 
cd ~/.ssh 
cat id_dsa.pub >> authorized_keys 
然后ssh localhost如果不需要密码就是设置成功了。 
如果不成功，检查authorized_keys和.ssh目录的权限 
chmod 644 authorized_keys 
cd ~ 
chmod 700 .ssh 
应该就可以了。
================================================================================
按照提示进行处理就可以了， 
dpkg --conigure -a
```

## 6、在linux命令下如何访问一个url？

```shell
1.elinks - lynx-like替代角色模式WWW的浏览器 
例如： 
elinks --dump http://www.baidu.com 
2.wget 这个会将访问的首页下载到本地 
[root@el5-mq2 ~]# wget http://www.baidu.com 
--2011-10-17 16:30:10-- http://www.baidu.com/ 
Resolving www.baidu.com... 119.75.218.45, 119.75.217.56 
Connecting to www.baidu.com|119.75.218.45|:80... connected. 
HTTP request sent, awaiting response... 200 OK 
Length: 8403 (8.2K) [text/html] 
Saving to: index.html' 
100%[==========================================================================================>] 8,403 --.-K/s in 0.01s 
2011-10-17 16:30:10 (648 KB/s) -index.html' saved [8403/8403] 
3.curl会显示出源码 
curl http://www.baidu.com/index.html 
4.lynx（这个以前在群里面见有人讨论过，但是没有尝试过，想用的话还需要下载软件） 
lynx http://www.baidu.com
```

## 7、安装Tomcat

http://www.51cto.com/art/200710/58374.htm

```shell
简单介绍Linux下安装Tomcat的步骤
http://os.51cto.com  2007-10-19 14:54  sixth  赛迪网  我要评论(1)
1 摘要：Tomcat是一个免费的开源的Serlvet容器，它是Apache基金会的Jakarta项目中的一个核心项目，由Apache，Sun和其它一些公司及个人共同开发而成。由于有了Sun的参与和支持，最新的Servlet和Jsp规范总能在Tomcat中得到体现。
2 标签：Linux  安装  Tomcat  JDK
Tomcat是一个免费的开源的Serlvet容器，它是Apache基金会的Jakarta项目中的一个核心项目，由Apache，Sun和其它一些公司及个人共同开发而成。由于有了Sun的参与和支持，最新的Servlet和Jsp规范总能在Tomcat中得到体现。
Tomcat是稳固的独立的Web服务器与Servlet Container，不过，其Web服务器的功能则不如许多更健全的Web服务器完整，如Apache Web服务器(举例来说，Tomcat没有大量的选择性模块)。不过，Tomcat是自由的开源软件，而且有许多高手致力于其发展。
在安装Tomcat之前需要安装j2sdk(Java 2 Software Development Kit)，也就是JDK
◆1、安装JDK的步骤如下：
1）下载j2sdk ，如jdk-6u1-linux-i586-rpm.bin
2）在终端中转到jdk-6u1-linux-i586-rpm.bin所在的目录，输入命令
#chmod +755 jdk-6u1-linux-i586-rpm.bin；//添加执行的权限。
3）执行命令
#./jdk-6u1-linux-i586-rpm.bin；//生成jdk-6u1-linux-i586.rpm的文件。
4）执行命令
#chmod +755 jdk-6u1-linux-i586.rpm；//给jdk-6u1-linux-i586.rpm添加执行的权限。
5）执行命令
#rpm –ivh jdk-6u1-linux-i586.rpm ； //安装jdk。
6）安装界面会出现授权协议，按Enter键接受，把jdk安装在/usr/java/jdk1.6.0_01。
7)设置环境变量，在 /etc/profile中加入如下内容(可以使用vi进行编辑profile)：
JAVA_HOME=/usr/java/jdk1.6.0_01 
CLASSPATH=$JAVA_HOME/lib:$JAVA_HOME/jre/lib 
PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin 
export PATH CLASSPATH JAVA_HOME
8)在终端执行命令java –version，jdk的版本为jdk1.6.0_01则表示jdk已成功安装。
◆2、安装Tomcat
1）下载apache-tomcat-6.0.10.tar.gz
2）#tar -zxvf apache-tomcat-6.0.10.tar.gz ；//解压
3）#cp -R apache-tomcat-6.0.10 /usr/local/tomcat ；//拷贝apache-tomcat-6.0.10到/usr/local/下并重命名为tomcat
4） /usr/local/tomcat/bin/startup.sh； //启动tomcat
显示 Using CATALINA_BASE: /usr/local/tomcat
Using CATALINA_HOME: /usr/local/tomcat
Using CATALINA_TEMDIR: /usr/local/tomcat/temp
Using JAVA_HOME: /usr/java/jdk1.6.0_01
到此tomcat已经安装完成，现在使用浏览器访问 http://localhost:8080，出现tomcat默认页面，说明已经安装成功。
```

## 8、Tomcat的启动与管理

```shell
tomcat是随机启动的，所以在开启服务器的时候要手动开启tomcat，不然没法访问（网上说可以设置随着服务器开启而开启，我还不会妮。。。）

1：找到tomcat安装路径
[root@localhost ~]# cd .. （返回上一级目录）
[root@localhost /]# ls        （列出该目录下的所有文件）
bin   dev  home  lib64       media  mnt  opt   root  selinux  sys       tmp  var 
boot  etc  lib   lost+found  misc   net  proc  sbin  srv      tftpboot  usr 
[root@localhost /]# cd var   （打开var这个文件夹）
[root@localhost var]# ls     （查看该文件的列表）
account   crash  empty  gdm       local  mail   opt       run    tux
arpwatch  cvs    ftp    kerberos  lock   named  preserve  spool  www
cache     db     games  lib       log    nis    racoon    tmp    yp 
（看来不在var文件夹中，只能返回上一级目录）
[root@localhost var]# cd .. 
[root@localhost /]# cd usr 
[root@localhost usr]# ls 
2.sql                     etc      java      lib64    sbin   tmp
apache-tomcat-7.0.14.tar  games    kerberos  libexec  share  tomcat7.0 
bin                       include  lib       local    src    X11R6 
[root@localhost usr]# cd tomcat7.0   （打开tomcat7.0这个文件夹）
[root@localhost tomcat7.0]# ls 
bin    hsperfdata_root  LICENSE  NOTICE         RUNNING.txt  webapps
conf  lib              logs     RELEASE-NOTES  temp         work 
[root@localhost tomcat7.0]# cd bin 
[root@localhost bin]# ls 
bootstrap.jar                 configtest.sh     setclasspath.sh  tomcat-native.tar.gz
catalina.bat                  cpappend.bat      shutdown.bat     tool-wrapper.bat
catalina.sh                   d:                shutdown.sh      tool-wrapper.sh
catalina-tasks.xml            digest.bat        startup.bat      version.bat
commons-daemon.jar            digest.sh         startup.sh       version.sh
commons-daemon-native.tar.gz  setclasspath.bat  tomcat-juli.jar 
[root@localhost bin]# ./ （当前目录，这里是做什么用的，不太清楚。。。）
-bash: ./: is a directory
[root@localhost bin]# ./startup.sh      （启动Tomcat）
Using CATALINA_BASE:   /usr/tomcat7.0
Using CATALINA_HOME:   /usr/tomcat7.0
Using CATALINA_TMPDIR: /usr/tomcat7.0
Using JRE_HOME:        /usr/java/jdk1.6.0_24
Using CLASSPATH:       /usr/tomcat7.0/bin/bootstrap.jar:/usr/tomcat7.0/bin/tomcat-juli.jar

启动成功

2：查看tomcat的日志
[root@localhost bin]# cd .. 
[root@localhost tomcat7.0]# ls 
bin   hsperfdata_root  LICENSE  NOTICE         RUNNING.txt  webapps
conf  lib              logs      RELEASE-NOTES  temp         work 
[root@localhost tomcat7.0]# cd logs  (打开日志文件夹)
[root@localhost logs]# ls 
catalina.2011-05-30.log      localhost.2011-06-28.log
catalina.out                 localhost_access_log.2011-06-27.txt
host-manager.2011-05-30.log  localhost_access_log.2011-07-07.txt 
（没用的不贴了）
[root@localhost logs]# tail -f catalina.out   （查看tomcat的日志文件）
```

## 9、重启Tomcat

```shell
Linux下Tomcat重新启动
在Linux系统下，重启Tomcat使用命令操作的！
首先，进入Tomcat下的bin目录
cd /usr/local/tomcat/bin
使用Tomcat关闭命令
./shutdown.sh
查看Tomcat是否以关闭
ps -ef|grep java
如果显示以下相似信息，说明Tomcat还没有关闭

root      7010     1  0 Apr19 ?        00:30:13 /usr/local/java/bin/java -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties -Djava.awt.headless=true -Dfile.encoding=UTF-8 -server -Xms1024m -Xmx1024m -XX:NewSize=256m -XX:MaxNewSize=256m -XX:PermSize=256m -XX:MaxPermSize=256m -XX:+DisableExplicitGC -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djava.endorsed.dirs=/usr/local/tomcat/endorsed -classpath /usr/local/tomcat/bin/bootstrap.jar -Dcatalina.base=/usr/local/tomcat -DcLinux下Tomcat重新启动
在Linux系统下，重启Tomcat使用命令操作的！
首先，进入Tomcat下的bin目录
cd /usr/local/tomcat/bin
使用Tomcat关闭命令
./shutdown.sh
查看Tomcat是否以关闭
ps -ef|grep java
如果显示以下相似信息，说明Tomcat还没有关闭

root      7010     1  0 Apr19 ?        00:30:13 /usr/local/java/bin/java -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties -Djava.awt.headless=true -Dfile.encoding=UTF-8 -server -Xms1024m -Xmx1024m -XX:NewSize=256m -XX:MaxNewSize=256m -XX:PermSize=256m -XX:MaxPermSize=256m -XX:+DisableExplicitGC -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djava.endorsed.dirs=/usr/local/tomcat/endorsed -classpath /usr/local/tomcat/bin/bootstrap.jar -Dcatalina.base=/usr/local/tomcat -Dcatalina.home=/usr/local/tomcat -Djava.io.tmpdir=/usr/local/tomcat/temp org.apache.catalina.startup.Bootstrap start

*如果你想直接干掉Tomcat，你可以使用kill命令，直接杀死Tomcat进程
kill -9 7010
然后继续查看Tomcat是否关闭
 ps -ef|grep java
如果出现以下信息，则表示Tomcat已经关闭
root      7010     1  0 Apr19 ?        00:30:30 [java] <defunct>
最后，启动Tomcat
 ./startup.sh
atalina.home=/usr/local/tomcat -Djava.io.tmpdir=/usr/local/tomcat/temp org.apache.catalina.startup.Bootstrap start

*如果你想直接干掉Tomcat，你可以使用kill命令，直接杀死Tomcat进程
 kill -9 7010
然后继续查看Tomcat是否关闭
 ps -ef|grep java
如果出现以下信息，则表示Tomcat已经关闭
root      7010     1  0 Apr19 ?        00:30:30 [java] <defunct>
最后，启动Tomcat
 ./startup.sh
```

## 10、Tomcat配置访问首页，访问目录

```
上回说到Tomcat在Linux下面的安装，今天来谈一谈tomcat的基本配置，打开tomcat的目录，其中有一个webapps目录和一个conf目录，这两个是比较重要的目录。
1.配置端口号：我Linux的机器没有IIS，所以80端口应该不会被占用，所以可以选择让tomcat来占用80端口，这样浏览器里面就不必输入冒号加端口号了，修改conf目录下的server.xml文件，将其中的8080修改为80即可。

2.添加虚拟目录：我们需要测试和发布自己的JSP工程，不能每次都打开tomcat默认的webapps文件夹往里面添加文件，而且，使用ubuntu－Linux的朋友在图像界面里在非自己的文件夹里面操作是很费事的，所以我们需要添加虚拟目录，比如我们现在有/home/newflypig/jspctest这个目录，我想将它设置为我的测试目录，第一部，需要有一个WEB－INF文件夹，我们到tomcat默认的/webapps/ROOT文件夹中拷贝一个过来，将WEB－INF文件夹中除了web.xml文件都删除了，我们仅仅需要这个web.xml就可以了。第二步，在conf目录下的server.xml添加：<Context path="/test" docBase="/home/newflypig/jsptest"  debug="0"   reloadable="true"   crossContext="true"/>主义这句话的添加位置，在最后一块的host前面添加。重启tomcat，可以访问http://localhost/test了。

3.设置listings，我们在测试阶段没必要每次都从index.jsp开始编写程序，可是在浏览器中输入的时候却很是不方便，默认情况下tomcat是为我们打开listings功能的，就是目录文件浏览功能，可是我这个版本一开始就关闭了，害我总以为刚开始配置虚拟目录出毛病，后来才发现是listings功能被禁用了，测试阶段我们可以打开这个功能，发布出去后就可以将这个功能关闭了，具体做法：conf/web.xml文件，修改其中的listings字段为true，默认为false，重启tomcat，可以看到虚拟目录下正常的一些文件列表了
```

## 11、TomCat日志查看

```shell
如何查看linux tomcat日志
 
1.切换到tomcat_home/logs目录下。
 
2.运行tail -f catalina.out即可查看日志。
```



## 12、LAMP兄弟连，安装教程MySql

检查mysql在linux是否存在

```shell
[root@localhost download]# rpm -qa | grep mysql
mysql-5.0.77-4.el5_4.2
```

卸载mysql

```shell
[root@localhost download]# rpm -e --nodeps mysql-5.0.77-4.el5_4.2
```

解压mysql-5.0.41.tar.gz

解压ncurses-5.6.tar.gz到指定的安装目录

编译ncures

是否启用共享文件，是否….

```shell
[root@localhost ncurses-5.6]# ./configure --with-shared --without-debug --without-ada --enable-overwrite
```

执行make

ubuntu下无法安装执行

```shell
sudo apt-get install libncurses5-dev
```

```shell
[root@localhost ncurses-5.6]# make
[root@localhost ncurses-5.6]# sudo make install
```

创建mysql组

```shell
[root@localhost download]# groupadd mysql
```

确认添加成功

```shell
[root@localhost mysql]# grep mysql /etc/group
```

添加用户到管理员组

```shell
[root@localhost mysql]# useradd -g mysql mysql
```

确认用户添加成功过

```shell
[root@localhost mysql]# grep mysql /etc/passwd
mysql:x:502:505::/home/mysql:/bin/bash
```

编译mySQl

```shell
[root@localhost mysql-5.0.41]# ./configure --prefix=/usr/install/mysql --with-extra-charesets=all
```

红色:安装的目录

执行-make --make install

生成配置文件

```shell
[root@localhost mysql-5.0.41]# cp support-files/my-medium.cnf /etc/my.cnf
```

授权表创建

```shell
[root@localhost mysql-5.0.41]# /usr/install/mysql/bin/mysql_install_db --user=mysql
```

改变所有文件所有者为root

```shell
[root@localhost mysql-5.0.41]# chown -R root /usr/install/mysql
```

把var改变为目录改变为mysql

```shell
[root@localhost mysql-5.0.41]# chown -R mysql /usr/install/mysql/var
```

改变所属于组为mysql

```shell
[root@localhost mysql-5.0.41]# chgrp -R mysql /usr/install/mysql
```

启动服务：

```shell
[root@localhost mysql-5.0.41]# /usr/install/mysql/bin/mysqld_safe --user=mysql &
```

查看Mysql的进程是否存在

```shell
[root@localhost mysql-5.0.41]# ps -le | grep mysql
4 S     0 18984  3531  0  85   0 -  1208 wait   pts/1    00:00:00 mysqld_safe
4 S   502 19008 18984  0  78   0 - 28552 stext  pts/1    00:00:00 mysqld
```

查看3306端口是否处于监听状态

```shell
[root@localhost mysql-5.0.41]# netstat -an | grep 3306
tcp        0      0 0.0.0.0:3306                0.0.0.0:*                   LISTEN   
```

查看mysql的版本信息

```shell
[root@localhost mysql-5.0.41]# /usr/install/mysql/bin/mysqladmin version
```

查看所有配置详细信息

```shell
[root@localhost mysql-5.0.41]# /usr/install/mysql/bin/mysqladmin variables
```

为mysql设置密码：

```shell
[root@localhost mysql-5.0.41]# /usr/install/mysql/bin/mysql -u root
```

```shell
mysql> SET PASSWORD FOR 'root'@'localhost'=PASSWORD('123456') ;
```

拷贝启动服务到系统目录

```shell
[root@localhost mysql-5.0.41]# cp /usr/install/mysql-5.0.41/support-files/mysql.server /etc/rc.d/init.d/mysql.d
```

改变所属者与所属组

```shell
[root@localhost mysql-5.0.41]# chown root.root /etc/rc.d/init.d/mysql.d
```

改变操作权限

```shell
[root@localhost mysql-5.0.41]# chmod 755 /etc/rc.d/init.d/mysql.d
```

将配置文件纳入linux管理体系中 ubuntu 没有chkconfig命令

```shell
[root@localhost mysql-5.0.41]# chkconfig --add mysql.d
```

检测mysql在每一级别的启动状态

```shell
[root@localhost mysql-5.0.41]# chkconfig --list mysql.d
mysql.d         0:关闭  1:关闭  2:启用  3:启用  4:启用  5:启用  6:关闭
```

设置mysql只在3级别自启动

```shell
[root@localhost mysql-5.0.41]# chkconfig --levels 245 musql.d off
```

配置环境变量
编辑root目录下的[root@localhost ~]# vi .bash_profile文件

注销—》登录

登录mysql



## 13、使用rpm将rpm包安装到指定目录

```shell
redhat/centos系统下将rpm包安装到指定目录
3 
4 |
5 浏览：0
6 |
7 更新：2014-07-20 23:22
redhat/centos等以rpm包为载体软件的linux系统，可以直接用命令：rpm –ivh xxx.rpm 默认安装rpm包，亦可以指定安装到某一目录下。为软件包指定安装目录：要加 -relocate 参数。
工具/原料
linux系统，本实验以redhat系统为准
方法/步骤
1
比如安装xxx.rpm包，以relocate 参数进行安装，安装到/opt/temp目录：
rpm -ivh --relocate /=/opt/temp xxx.rpm；
以prefix进行安装：
rpm -ivh --prefix= /opt/temp  xxx.rpm
```

## 14、配置samba服务器超好用

网址：http://blog.csdn.net/linglongwunv/article/details/5212875

```shell
注意：本文的原则是只将文件共享应用于内网服务器，并让将要被共享的目录拥有充分的读写权限属性，读者可顺着本文的思路完成基本配置流程，如需复杂读写权限功能，请参考此文章http://blog.csdn.net/linglongwunv/archive/2010/01/19/5213337.aspx。
1、# yum -y install samba
使用yum命令安装samba，加入-y参数，如遇询问自动选择y，全自动下载并安装samba，此过程需要一点时间。
2、# rpm -qa | grep samba
检查samba服务包的安装情况，会显示类似如下两个包：
samba-common-3.0.33-3.7.el5_3.1   //服务器和客户端均需要的文件
samba-3.0.33-3.7.el5_3.1                //服务器端文件
3、# whereis samba
由于是yum安装，可以用此命令查看samba安装位置，得到类似如下内容：
samba: /etc/samba /usr/lib/samba /usr/share/samba /usr/share/man/man7/samba.7.gz
4、# vi /etc/samba/smb.conf
根据步骤3得知smb.conf的位置，配置samba：
（1）[global]       找到全局设置标签，在下面进行配置
workgroup = MYGROUP      找到此行，改为workgroup = WORKGROUP，这里以 Windows XP 默认的“WORKGROUP”为例
; hosts allow = 192.168.1. 192.168.2. 127.      找到此行，去掉行首的“;”，并制定访问限制改为hosts allow = 192.168.0. 127.，指定内网IP地址及本地，只允许这两种情况的访问
（2）配置最简单访问目录几个基本属性：
[share]      windows客户端查看时看到的文件夹名
path = /var/samba/share      共享目录位置，要系统中存在的目录，也可以配置完再创建
read only = no
public   = yes
5、给配置的共享目录设置权限：
# mkdir /var/samba/share      如刚才配置的共享目录不存在则创建
# chown -R nobody. /var/samba/share      设置共享目录归属为 nobody 
# chmod 777 /var/samba/share      将共享目录属性设置为 777
6、# smbpasswd -a username      将linux系统已存在用户 username（例）加入到 Samba 用户数据库，windows访问samba共享目录时需要输入此用户名和密码
New SMB password:      在此输入密码
Retype new SMB password:      重复密码
7、# service smb start
由于是yum安装可用此命令启动samba，若想开机自启动samba服务，请参考此文章http://blog.csdn.net/linglongwunv/archive/2010/01/13/5186968.aspx
8、若启动成功，最简单的适合内网使用的samba已配置好。卸载samba请参考此文章http://blog.csdn.net/linglongwunv/archive/2010/01/19/5212868.aspx
9、从Windows 客户端连接到Samba 服务器，即客户端使用samba的方法可参考此文章http://blog.csdn.net/linglongwunv/archive/2010/01/19/5212919.aspx
```

在windows下 win + R 输入 ：服务器IP即可打开，可以映射网络磁盘

## 15、Apache服务器配置

### 15.1、配置文件

配置文件目录： 
ubuntu:/etc/httpd/httpd.conf 
主页目录

```xml
1.         DocumentRoot "/home/sambaserver/share/project/web/yhywork/"
2.         <Directory "/home/sambaserver/share/project/web/yhywork/">
```

主页文件

```xml
1. # DirectoryIndex: sets the file that Apache will serve if a directory
2. # is requested.
3. #
4. <IfModule dir_module>
5.     DirectoryIndex index.html
6. </IfModule>
```

cgi-bin

```shell
1. <IfModule alias_module>
2.     #
3.     # Redirect: Allows you to tell clients about documents that used to 
4.     # exist in your server's namespace, but do not anymore. The client 
5.     # will make a new request for the document at its new location.
6.     # Example:
7.     # Redirect permanent /foo http://www.example.com/bar
8. 
9.     #
10.     # Alias: Maps web paths into filesystem paths and is used to
11.     # access content that does not live under the DocumentRoot.
12.     # Example:
13.     # Alias /webpath /full/filesystem/path
14.     #
15.     # If you include a trailing / on /webpath then the server will
16.     # require it to be present in the URL.  You will also likely
17.     # need to provide a <Directory> section to allow access to
18.     # the filesystem path.
19. 
20.     #
21.     # ScriptAlias: This controls which directories contain server scripts. 
22.     # ScriptAliases are essentially the same as Aliases, except that
23.     # documents in the target directory are treated as applications and
24.     # run by the server when requested rather than as documents sent to the
25.     # client.  The same rules about trailing "/" apply to ScriptAlias
26.     # directives as to Alias.
27.     #
28.     ScriptAlias /cgi-bin/ "/home/sambaserver/share/project/web/yhywork/cgi-bin/"
29. 
30. </IfModule>
1. <Directory "/home/sambaserver/share/project/web/yhywork/cgi-bin/">
2.     AllowOverride None
3.     Options None
4.     Order allow,deny
5.     Allow from all
6.     AddType text/html .shtml .htm .html
7.     AddType image/png .png
8.     AddType image/jpeg .jpg
9. </Directory>
```

增加cgi执行类型

1. AddHandler cgi-script .cgi .pl .py .sh
   注意要给cgi程序增加可执行的权限



