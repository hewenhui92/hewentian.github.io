---
title: Linux 学习笔记
date: 2017-09-12 20:41:37
tags: Linux
categories: Linux
---

在安装ubuntu的过程中的分区顺序: /boot -> / -> swap:

	/boot	primary		512M
	/		primary		剩下的容量减去内存2倍
	swap	logic		内存2倍

Ubuntu安装ssh时出现软件包 openssh-server 还没有可供安装的候选者错误

错误如下：

sudo apt-get install opensshserver正在读取软件包列表...

完成正在分析软件包的依赖关系树正在读取状态信息...

完成现在没有可用的软件包 openssh-server，

但是他被其他的软件包引用了这可能意味着这个缺失的软件包可能已被废弃，或者只能在其他发布源中找到

E:软件包 openssh-server 还没有可供安装的候选者

解决方案：分析原因是我们的apt-get没有更新，当然如果你的是最新的系统不用更新也行，但是我相信很多人都是需要更新的吧，操作命令如下：
``` bash
$ sudo apt-get update
```
更新完毕后执行：
``` bash
$ sudo apt-get install openssh-server
```
最后我们用命令ps -e|grep ssh 来看下open-server安装成功没有，如果出现如下截图红色标出的部分，说明安装成功了。
``` bash
$ ps -e|grep ssh
```

在ubuntu中的vi编辑器中怎么使用
默认情况下ubuntu上也安装有vi但是奇怪的是这个vi是vim-common版本，基本上用不了所以要先把这个版本的vi卸载掉才可以，卸载命令是
``` bash
$ sudo apt-get remove vim-common
```
卸载成功之后接着执行 sudo apt-get install vim,安装好之后就能使用了

### ubuntu下useradd与adduser区别，新建用户不再home目录下
useradd username不会在/home下建立一个文件夹username
adduser username会在/home下建立一个文件夹username
useradd -m username跟adduser一样，可以建立一个文件夹username

永久性删除用户账号
``` bash
$ userdel peter
```

还有的就是文件编码的问题，在windows下默认都是用ANSI格式编码，但是在ubuntu下面可能会是其他格式的编码，如果两个系统的编码不一致，就会产生乱码。
最好是都采用UTF-8格式编码


ubuntu root默认密码（初始密码）
ubuntu安装好后，root初始密码（默认密码）不知道，需要设置。

1、先用安装时候的用户登录进入系统

2、输入：
``` bash
sudo passwd
```
按回车

3、输入新密码，重复输入密码，最后提示passwd：password updated sucessfully

此时已完成root密码的设置

4、输入：
``` bash
$ su root
```
切换用户到root试试.......




如果执行./configure无法执行，
要安装：
``` bash
$ sudo apt-get install build-essential
```

在ubuntu软件源里zlib和zlib-devel叫做zlib1g zlib1g.dev
``` bash
$ sudo apt-get install zlib1g
$ sudo apt-get install zlib1g.dev
```

直接输入上述命令后还是不能安装。这就要求我们先装ruby.
在ubuntu里，zlib叫zlib1g，相应的zlib-devel叫zlib1g.dev。默认的安装源里没有zlib1g.dev。要在packages.ubuntu.com上找。
``` bash
$ sudo apt-get install ruby
```

然后再装zlib1g-dev就可以了
``` bash
$ sudo apt-get install zlib1g-dev
```

解决依赖包openssl安装，命令：
[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
``` baah
$ sudo apt-get install openssl libssl-dev  
```

解决依赖包pcre安装，命令：
[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
``` bash
$ sudo apt-get install libpcre3 libpcre3-dev  
```

解决依赖包zlib安装，命令：
[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
``` bash
$ sudo apt-get install zlib1g-dev  
```

### 当你安装完ubuntu后，安装五笔输入法：
1.打开命令行输入：
``` bash
$ sudo apt-get install fcitx-table-wubi
```
安装完后，重启（也可以在下面的设置完毕之后，再重启）

2.`System Settings -> language support` 在 Keyboard input method system 中选中 fcitx
然后点击 Apply System-Wide, 然后重启

3.重启后，`System Settings -> Text Entry`, 中输入 Wubi(Fcitx)，这样，就完成了五笔输入法的安装。


### ubuntu 安装 Robo3t 出现 Available platform plugins are: xcb.
解决办法,打开终端输入一下命令
``` bash
$ mkdir ~/robo-backup
$ mv robo3t-1.1.1-linux-x86_64-c93c6b0/lib/libstdc++* ~/robo-backup/
```

输入命令之后会在robo-backup中出现两个文件 libstdc++.so.6 和 libstdc++.so.6.0.22，然后启动robo3t就ok了

其实，也可以將其删掉，只要你还有压缩包，即可。


### ubuntu 下 Eclipse 中 syso 快捷键 Alt + / 不能使用的问题
在`window -> Preferences -> general -> keys`中，
找到 content asist 修改下边值
Binding 改成 Alt+/   		输入的时候要注意，长按Alt键，然后按下/键即可
When 改为 Editing Text 
ok.


### ubuntu 使用`switchHosts`修改host后启动`eclipse`项目报错
``` java
错误: 抛出异常错误: `java.net.MalformedURLException: Local host name unknown: java.net.UnknownHostException:
localhost: hewentian-Lenovo-IdeaPad-Y470`: 未知的名称或服务
Error: Exception thrown by the agent : java.net.MalformedURLException: Local host name unknown: 
java.net.UnknownHostException: localhost: hewentian-Lenovo-IdeaPad-Y470: Name or service not known
```
原因是/etc/hosts文件里没有主机名为：hewentian-Lenovo-IdeaPad-Y470，解决方法就是在`switchHosts`中增加如下代码：
``` xml
127.0.0.1  hewentian-Lenovo-IdeaPad-Y470
localhost  hewentian-Lenovo-IdeaPad-Y470
```


### Linux查看程序端口占用情况
``` bash
$ netstat -apn | grep 8761
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp6       0      0 :::8761                 :::*                    LISTEN      24831/java      
tcp6       0      0 127.0.0.1:49958         127.0.0.1:8761          ESTABLISHED 24824/java      
tcp6       0      0 127.0.0.1:50494         127.0.0.1:8761          TIME_WAIT   -               
tcp6       0      0 127.0.0.1:50502         127.0.0.1:8761          TIME_WAIT   -               
tcp6       0      0 127.0.0.1:50484         127.0.0.1:8761          TIME_WAIT   -               
tcp6       0      0 127.0.0.1:8761          127.0.0.1:49958         ESTABLISHED 24831/java      
unix  3      [ ]         STREAM     CONNECTED     2887618  21883/chrome        
unix  3      [ ]         STREAM     CONNECTED     2887619  22047/chrome --type 
```
其中最后一栏是PID/Program name 

### ubuntu下安装notepadqq
安装过程中参考了如下文章：
https://www.linuxtechi.com/notepadqq-notepad-for-ubuntu-linux/

Step:1 Add Ubuntu PPA Repository

‘notpadqq‘ package is not available in the default Ubuntu repository, we will add Ubuntu PPA repository using below command.
``` bash
$ sudo add-apt-repository ppa:notepadqq-team/notepadqq
```
Step:2 Refresh the Repositories using below apt-get command.
``` bash
$ sudo apt-get update
```
Step:3 Install notepaddqq Debian package
``` bash
$ sudo apt-get install notepadqq
```
成功。




### ubuntu Navicat for MySQL 安装以及破解方案  

首先上官网上下载LINUX版本： http://www.navicat.com/download/navicat-for-mysql

1. 下载 navicat120_mysql_en_x64.tar.gz 文件

2. 下载后解压tar文件
``` bash
tar -xzvf /home/hewentian/Downloads/navicat120_mysql_en_x64.tar.gz
```
3. 解压后  进入解压后的目录运行命令：
``` bash
./start_navicat   
```
这样OK啦

连接上数据库后里面的中文数据是乱码,把Ubuntu的字符集修改为`zh_CN.utf8`就行了,修改方法:

1. 查看当前系统的字符集：echo $LANG
2. 查看系统支持的字符集: locale -a  
3. 修改字符集: export LANG=zh_CN.utf8  
4. 打开start_navicat文件，会看到 export LANG="en_US.UTF-8" 将这句话改为 export LANG="zh_CN.UTF-8"

经过以上修改，我的还是有中文乱码。

破解试用期方案：
第一次执行start_navicat时，会在用户主目录下生成一个名为`.navicat64`的隐藏文件夹，只要重新删除该文件即可重新计算14天试用期。
``` bash
$ cd /home/hewentian
$ ls -a 				# 显示隐藏文件内的所有文件
$ rm -rf .navicat64 	# 删除该文件
$ 						# 或者删除此目录下的system.reg文件，但是没有效果
```
把文件夹删除后，下次启动navicat 会重新生成此文件，14天试用期会按新的时间开始计算。


### linux中解压rar文件
可以先执行如下命令，看系统是否已经安装 rar 命令
``` bash
hewentian@hewentian-Lenovo-IdeaPad-Y470:~/Downloads$ rar
The program 'rar' is currently not installed. You can install it by typing:
sudo apt install rar
```
如果输出如上面，则`Linux`中未安装`rar`命令，按提示安装即可
``` bash
hewentian@hewentian-Lenovo-IdeaPad-Y470:~/Downloads$ sudo apt install rar
[sudo] password for hewentian: 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Suggested packages:
  unrar
The following NEW packages will be installed:
  rar
0 upgraded, 1 newly installed, 0 to remove and 142 no
```
等待安装完成即可，验证是否成功安装，输入`rar`即可，看到如下输出，即证明安装成功。
``` bash
hewentian@hewentian-Lenovo-IdeaPad-Y470:~/Downloads$ rar

RAR 5.30 beta 2   Copyright (c) 1993-2015 Alexander Roshal   4 Aug 2015
Trial version             Type RAR -? for help

Usage:     rar <command> -<switch 1> -<switch N> <archive> <files...>
               <@listfiles...> <path_to_extract\>

<Commands>
  a             Add files to archive
  c             Add archive comment
  ch            Change archive parameters
  cw            Write archive comment to file
  d             Delete files from archive

```
解压方法很简单：
``` bash
hewentian@hewentian-Lenovo-IdeaPad-Y470:~/Downloads$ rar x 要解压的文件名.rar
```
这将压缩包解压在当前目录下



### 当安装软件的时候出现如下错误的时候
``` bash
linux-image-extra-4.10.0-38-generic depends on linux-image-4.10.0-38-generic; however:
  Package linux-image-4.10.0-38-generic is not configured yet.
```
解决方法如下：
``` bash
$ sudo dpkg --configure -a
```


### 在Ubuntu上检查一个软件包是否安装

要检查特定的包，比如firefox是否安装了，使用这个命令：
``` bash
$ dpkg -s firefox
```
同样，你可以使用dpkg-query 命令。这个命令会有更好的输出，当然，你可以用通配符。
``` bash
$ dpkg-query -l firefox
```
要列出你系统中安装的所有包，输入下面的命令：
``` bash
$ dpkg --get-selections
```
你同样可以通过grep来过滤割到更精确的包。比如，我想要使用dpkg命令查看系统中安装的gcc包：
``` bash
$ dpkg --get-selections | grep gcc
```

### ubuntu 解压zip文件出现乱码
由于zip格式中并没有指定编码格式，Windows下生成的zip文件中的编码是GBK/GB2312等，因此，导致这些zip文件在Linux下解压时出现乱码问题，因为Linux下的默认编码是UTF8

有2种方式解决问题：

1. 通过unzip行命令解压，指定字符集
unzip -O CP936 {要解压的文件名}.zip (用GBK, GB18030也可以)

2. 在环境变量中，指定unzip参数，总是以指定的字符集显示和解压文件
/etc/environment中加入2行
UNZIP="-O CP936"
ZIPINFO="-O CP936"


### 在 Ubuntu 上面远程连回 Windows 7
1. 首先需要开启 Windows 7 上的远程桌面: 打开[控制面板] -> [管理工具] -> [服务] -> [Terminal Services], 开启该服务（有可能找不到）;
2. 然后右击 [我的电脑] -> 选择[属性] -> 远程设置 -> 远程 -> 选择远程协助中的 [允许远程协助连接这台计算机]，下面的远程桌面选择 [允许运行任意版本远程桌面的计算机连接];
3. 有可能还需要关闭[防火墙];
4. 在 Ubuntu 上面使用如下命令连回 Windows 7; 
``` bash
$ rdesktop {Windows7_IP}
或者 
$ rdesktop {Windows7_IP} -f -u {YOUR_LOGIN_NAME} -p {YOUR_PASSWD} 
# -f 全屏，直接输入用户名和密码, 以全屏方式进入 windows 的退出方式是 [开始] -> [断开连接]
```

### Linux下的任务调度分为两类，系统任务调度和用户任务调度。
1. 系统任务调度：系统周期性所要执行的工作，比如写缓存数据到硬盘、日志清理等。在/etc目录下有一个crontab文件，这个就是系统任务调度的配置文件。
/etc/crontab文件包括下面几行：
``` bash
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
```

2. 用户任务调度：用户定期要执行的工作，比如用户数据备份、定时邮件提醒等。用户可以使用 crontab 工具来定制自己的计划任务。所有用户定义的crontab 文件都被保存在 /var/spool/cron目录中。其文件名与用户名一致

查看crontab服务状态：
``` bash
$ /etc/init.d/cron status  # 可用的参数 force-reload | reload | restart | start | status | stop
```
### crontab 的用法
``` bash
usage:	crontab [-u user] file
	crontab [ -u user ] [ -i ] { -e | -l | -r }
		(default operation is replace, per 1003.2)
	-e	(edit user's crontab)
	-l	(list user's crontab)
	-r	(delete user's crontab)
	-i	(prompt before deleting user's crontab)
```
例子如下：
``` bash
$ crontab -e
```
在打开的文件中输入如下内容，这将会在每分钟往文件中输出时间
``` bash
*/1 * * * * /bin/date >> /home/hewentian/Documents/a.txt
```

在资源浏览器 [Connect to Server] 窗口输入：smb://需要访问的机器IP
等同于Windows下输入：\\IP


### curl模拟http发送get或post请求
参照：http://www.voidcn.com/blog/Vindra/article/p-4917667.html

1. get请求 
curl "http://www.baidu.com"  如果这里的URL指向的是一个文件或者一幅图都可以直接下载到本地
curl -i "http://www.baidu.com"  显示全部信息
curl -l "http://www.baidu.com" 只显示头部信息
curl -v "http://www.baidu.com" 显示get请求全过程解析

wget "http://www.baidu.com"也可以

2. post请求
curl -d "param1=value1&param2=value2" "http://www.baidu.com"

3. json格式的post请求
curl -l -H "Content-type: application/json" -X POST -d '{"phone":"13800138000","password":"passwd"}' http://domain/apis/users.json

### 在ubuntu上面安装 maven
``` bash
$ su root 	# 切换到 root 用户
$ cd /home/hewentian/Downloads	# 进入到 maven 下载的目录
$ tar xzvf apache-maven-3.3.9-bin.tar.gz
# cd /usr/local
# mv /home/hewentian/Downloads/apache-maven-3.3.9 ./
$# 在 /etc/profile 文件中添加 maven 路径
# vi /etc/profile
$ # 在打开的文件中添加如下代码
# add maven
export M2_HOME=/usr/local/apache-maven-3.3.9
export PATH=$M2_HOME/bin:$PATH

保存，并退出，执行如下语句，验证安装结果
$ sour$ce /etc/profile
$ mvn -version

$ # 如无意外，你将看到如下输出
Maven home: /usr/local/apache-maven-3.3.9
Java version: 1.8.0_102, vendor: Oracle Corporation
Java home: /usr/local/java/jdk1.8.0_102/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.10.0-28-generic", arch: "amd64", family: "unix"
```

### ubuntu 16.04 安装 google-chrome-stable_current_amd64.deb 方法
``` bash
$ cd {google-chome所在目录}
$ sudo dpkg -i google-chrome-stable_current_amd64.deb
$ # 如果安装过程输出如下错误
$ # google-chrome-stable depends on libappindicator1; however:
$ # 那么执行如下命令
$ sudo apt-get -f install libappindicator1 libindicator7
$ # 然后再执行原来的安装命令
$ sudo dpkg -i google-chrome-stable_current_amd64.deb
```

### Ubuntu 16.04 关闭笔记本触摸板
1. 禁用触摸板
``` bash
$ sudo rmmod psmouse
```

2. 重启触摸板
``` bash
$ sudo modprobe psmouse
```

### linux下查看某命令的具体位置使用which，如：
``` bash
$ which ab
```


### 在Linux/unix 平台上的`sqlplus`中，如果输错了字符，要想删除，习惯性的按下`backspace`键后，发现非但没有删除想要删掉的字符，还多出了两个字符^H。当然，我们可以同时按下`ctrl+backspace`键来删除，但对于习惯了用`backspace`来删除的用户，这样很不爽。这可以通过修改tty终端的设置来实现`backspace`删除功能。通过使用stty命令，就可以查看或者修改终端的按键设置。
例如，设置backspace为删除键：
``` bash
$ stty erase ^h
```
如果要改回使用ctrl+backspace为删除键
``` bash
$ stty erase ^?
```
如果需要重启后自动设置终端，可以将上述命令加入到profile中。可以通过stty -a命令来查看所有的终端设置。下面是在linux下执行的输出：
``` bash
$ stty -a
$ cat /etc/os-release
```

### 关于Linux中的so文件
1. so文件就是通常说的动态链接库，就跟windows下的dll文件差不多；
2. ko是内核模块文件，驱动之类的啥的；
3. 不过在linux系统下文件的后缀多数情况下只是个标识，有可能代表不了文件的真实属性的。


### linux下采用LD_PRELOAD机制动态修改方法和注入代码
LD_PRELOAD是Linux下的一个环境变量，动态链接器在载入一个程序所需的所有动态库之前，
首先会载入LD_PRELOAD环境变量所指定的动态库。运用这个机制，我们可以修改/替换已有动态库中的方法，
加入我们自己的逻辑，从而改变程序的执行行为。不过该方法只对动态链接的程序有效，对静态链接的程序无效。


### linux下面查看文件的MD5值，用命令：md5sum
用法一，产生MD5值：
``` bash
$ md5sum [文件]
```
用法二，从文件中读取MD5的校验值并予以检查，要求同目录下面要有如下两个文件：

	solr-6.5.0.zip
	solr-6.5.0.zip.md5

命令如下：
``` bash
$ md5sum -c solr-6.5.0.zip.md5
如果相同，输出：
solr-6.5.0.zip: OK
```
md5值重定向将生成md5值重定向到指定的文件，通常文件的扩展名我们会命为.md5
``` bash
$ md5sum data > data.md5
$ md5sum data
0a6de444981b68d6a049053296491e49  data

$ cat data.md5 
0a6de444981b68d6a049053296491e49  data
```

### linux查找一个目录的命令
``` bash
查找目录：find /（查找范围） -name '查找关键字' -type d
```

### Linux查找含有某字符串的所有文件
	grep -rn "hello,world!" *
 
	* : 表示当前目录所有文件，也可以是某个文件名
	-r 是递归查找
	-n 是显示行号
	-R 查找所有文件包含子目录
	-i 忽略大小写
