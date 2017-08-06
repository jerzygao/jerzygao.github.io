---
layout: post
category: notes
title:  "Linux常用命令"
tags: [linux]
---
* [1. 系统相关](#system)

* [2. 压缩相关](#tar)

* [3. mysql相关](#mysql)

* [4. vim相关](#vim)

* [5. 重命名移动文件文件夹命令](#mv)

* [6. 文件文本处理相关](#file)

* [7. crontab定时任务](#crontab)

<!-- more -->
# 系统相关  {#system}

## 查看系统磁盘使用情况，各个文件大小
**df -Th**
>-h  参数 -h 表示使用「Human-readable」的输出 -T显示磁盘类型

**du -h --max-depth=1 /data/***
>max-depth=n n为数字 表示递归遍历n层目录，/data/*为遍历的文件夹

## 查看系统内存，CPU

CPU  
lscpu  
grep "model name" /proc/cpuinfo | cut -f2 -d:  
CPU位数  
getconf LONG_BIT  
内存 单位M  
dmidecode -t memory | grep Size  
使用 free 命令可以查看当前系统的内存信息，要查看内存的总数和空闲空间时，大家可使用 free 命令：  
free –m  
lspci  
列出所有连接到 PCI 总线的详细信息，例如：显卡、网卡、USB 接口及 SATA 控制器等设备  
lshw  
是一个通用工具，该工具可以执行多个硬件如：CPU、内存、USB 控制器及磁盘等详细信息。lshw 在执行之后会自动提取不同“/proc”文件中的信息。

## 修改密码

passwd root 修改root密码

## 重启与关机

重启命令：

1、reboot

2、shutdown -r now 立刻重启(root用户使用)

3、shutdown -r 10 过10分钟自动重启(root用户使用)

4、shutdown -r 20:35 在时间为20:35时候重启(root用户使用)

关机命令：

1、halt   立刻关机

2、poweroff  立刻关机

3、shutdown -h now 立刻关机(root用户使用)

4、shutdown -h 10 10分钟后自动关机


# 压缩相关  {#tar}

## 常有压缩解压命令列表

.tar  
解包：tar xvf FileName.tar  
打包：tar cvf FileName.tar DirName  
（注：tar是打包，不是压缩！）  
———————————————  
.gz  
解压1：gunzip FileName.gz  
解压2：gzip -d FileName.gz  
压缩：gzip FileName  

.tar.gz 和 .tgz  
解压：tar zxvf FileName.tar.gz  
压缩：tar zcvf FileName.tar.gz DirName  
———————————————  
.bz2  
解压1：bzip2 -d FileName.bz2  
解压2：bunzip2 FileName.bz2  
压缩： bzip2 -z FileName  

.tar.bz2  
解压：tar jxvf FileName.tar.bz2  
压缩：tar jcvf FileName.tar.bz2 DirName  
———————————————  
.bz  
解压1：bzip2 -d FileName.bz  
解压2：bunzip2 FileName.bz  
压缩：未知  

.tar.bz  
解压：tar jxvf FileName.tar.bz  
压缩：未知  
———————————————  
.Z  
解压：uncompress FileName.Z  
压缩：compress FileName  
.tar.Z  

解压：tar Zxvf FileName.tar.Z  
压缩：tar Zcvf FileName.tar.Z DirName  
———————————————  
.zip  
解压：unzip FileName.zip  
压缩：zip FileName.zip DirName  
———————————————  
.rar  
解压：rar x FileName.rar  
压缩：rar a FileName.rar DirName  
———————————————  
.lha  
解压：lha -e FileName.lha  
压缩：lha -a FileName.lha FileName  
———————————————  
.rpm  
解包：rpm2cpio FileName.rpm | cpio -div  
———————————————  
.deb  
解包：ar p FileName.deb data.tar.gz | tar zxf -  


## tar命令

-c: 建立压缩档案  
-x：解压  
-t：查看内容  
-r：向压缩归档文件末尾追加文件  
-u：更新原压缩包中的文件  

这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。下面的参数是根据需要在压缩或解压档案时可选的。

-z：有gzip属性的  
-j：有bz2属性的  
-Z：有compress属性的  
-v：显示所有过程  
-O：将文件解开到标准输出  

下面的参数-f是必须的

-f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

**tar -cf all.tar *.jpg**

这条命令是将所有.jpg的文件打成一个名为all.tar的包。-c是表示产生新的包，-f指定包的文件名。

 **tar -rf all.tar *.gif**

这条命令是将所有.gif的文件增加到all.tar的包里面去。-r是表示增加文件的意思。

**tar -uf all.tar logo.gif**

这条命令是更新原来tar包all.tar中logo.gif文件，-u是表示更新文件的意思。

**tar -tf all.tar**

这条命令是列出all.tar包中所有文件，-t是列出文件的意思

**tar -xf all.tar**

这条命令是解出all.tar包中所有文件，-t是解开的意思


### tar压缩

**tar -cvf jpg.tar *.jpg** //将目录里所有jpg文件打包成tar.jpg

**tar -czf jpg.tar.gz *.jpg**   //将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz

**tar -cjf jpg.tar.bz2 *.jpg** //将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2

**tar -cZf jpg.tar.Z *.jpg**   //将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z

**rar a jpg.rar *.jpg** //rar格式的压缩，需要先下载rar for linux

**zip jpg.zip *.jpg** //zip格式的压缩，需要先下载zip for linux


### tar解压

tar -xvf file.tar //解压 tar包

tar -xzvf file.tar.gz //解压tar.gz

tar -xjvf file.tar.bz2   //解压 tar.bz2

tar -xZvf file.tar.Z   //解压tar.Z

unrar e file.rar //解压rar

unzip file.zip //解压zip

## 总结

1、*.tar 用 tar -xvf 解压

2、*.gz 用 gzip -d或者gunzip 解压

3、\*.tar.gz和\*.tgz 用 tar -xzf 解压

4、*.bz2 用 bzip2 -d或者用bunzip2 解压

5、*.tar.bz2用tar -xjf 解压

6、*.Z 用 uncompress 解压

7、*.tar.Z 用tar -xZf 解压

8、*.rar 用 unrar e解压

9、*.zip 用 unzip 解压

# mysql相关  {#mysql}

## 启动方式

1、使用 service 启动：service mysqld start

2、使用 mysqld 脚本启动：/etc/inint.d/mysqld start

3、使用 safe_mysqld 启动：safe_mysqld&

## 停止

1、使用 service 启动：service mysqld stop

2、使用 mysqld 脚本启动：/etc/inint.d/mysqld stop

3、mysqladmin shutdown

## 重启

1、使用 service 启动：service mysqld restart

2、使用 mysqld 脚本启动：/etc/inint.d/mysqld restart

# vim相关   {#vim}

## 保存命令

按ESC键 跳到命令模式，然后：  
:w   保存文件但不退出vi  
:w file 将修改另外保存到file中，不退出vi  
:w!   强制保存，不推出vi  
:wq  保存文件并退出vi  
:wq! 强制保存文件，并退出vi  
q:  不保存文件，退出vi  
:q! 不保存文件，强制退出vi  
:e! 放弃所有修改，从上次保存文件开始再编辑  

## 格式化命令

gg 至文件头，G至文件尾  
命令模式直接输入gg=G格式化文件  
全选文本按顺序如下命令  
gg  
v  
G  

yy复制游标所在行整行。或大写一个Y。  
2yy或y2y复制两行。  
y^复制至行首，或y0。不含游标所在处字元。  
y$复制至行尾。含游标所在处字元。  
yw复制一个word。  
y2w复制两个字（单词）。  
yG复制至档尾。  
y1G复制至档首。  
p小写p代表贴至游标后（下）。  
P大写P代表贴至游标前（上）。  
如果只是想使用系统粘贴板的话直接在输入模式按Shift+Inset就可以了  

x &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;删除当前光标下的字符  
dw &nbsp;&nbsp;删除光标之后的单词剩余部分。  
d$ &nbsp;&nbsp;删除光标之后的该行剩余部分。  
dd &nbsp;&nbsp;删除当前行。  
c &nbsp;&nbsp;&nbsp;&nbsp;功能和d相同，区别在于完成删除操作后进入INSERT MODE  
cc &nbsp;&nbsp;也是删除当前行，然后进入INSERT MODE  

# 重命名 移动文件文件夹命令    {#mv}

inux下重命名文件或文件夹的命令mv既可以重命名，又可以移动文件或文件夹.

例子：将目录A重命名为B

mv A B

例子：将/a目录移动到/b下，并重命名为c

mv /a /b/c

# 文件文本处理相关   {#file}

**sed -i 's/gaozhe/superman/g' test.txt**

>将test.txt文件中所有的gaozhe替换为superman -i表示修改源文件

**sed -n '5,10p' filename**

>只查看文件的第5行到第10行

**cat test.log |head -n 8|tail -n +3** 第3行到第8行

**cat test.log |head -n 8|tail -n 3** 第6行到第8行
>tail -n 3：显示最后3行；
 tail -n +3：从3行开始显示，显示3行以后的；
 head -n 3：显示前面3行；


# crontab定时任务服务    {#crontab}

详细说明见：
[详细说明](http://www.cnblogs.com/peida/archive/2013/01/08/2850483.html)
