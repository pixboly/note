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
export PATH="变量路径
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
