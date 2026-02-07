## 本书结构

**第一部分**：Linux				 1-10

**第二部分**：编写shell脚本		   11-16

**第三部分**：深入shell脚本		   17-23

**第四部分**：shell脚本的显示应用	24-25



pip install ... -i https://pypi.tuna.tsinghua.edu.cn/simple



### 2

虚拟控制台：tty(teletypewriter)



### 3

#### 3.4

cd pwd  .   ..   /

(print working directory)

 当前目录：。

当前父目录：。。

#### 3.5

ls -aFl

文件的类型：/  *  .

标准通配符：?  *  []  !

#### 3.6

创建文件：touch

复制文件：cp  source  destination	cp -i	cp -R

命令行补全：Tab

链接文件：ln source destination（硬）ln -s f1 f2（软）

文件移动：mv so de	mv -i

文件移除(removing)：rm so 	rm -i	rm -f

#### 3.7

创建目录：mkdir	mkdir -p a/b/c	ls -d

删除目录：rmdir	-i	-r	-f

转移目录：mv source_dir destination_dir

#### 3.8：查看文件内容

查看文件类型：file

查看文件：

​	整个：cat	more	less	-n	-b

​	部分：tail	head	-n	-f(only tail)



### 4.更多的shell命令

Linux系统管理员：

跟踪运行在系统中的程序（难点：定义究竟到什么程度是高负载），知道何时以及如何结束一个进程（进程间通过信号来通信）

检测系统磁盘的使用情况

从日志文件中读取并处理数据



#### 4.1监测程序

探查进程：

特定时间点：ps	Unix风格(OSRZT)+GNU风格

实时：top

结束进程：

kill 进程信号	-s

pkill 程序名（可用通配符）

#### 4.2监测磁盘空间

文件系统类型：vfat	ntfs	exfat	iso9660

挂载存储设备：

mount -t type device directory

umount  [directory | divece ]

df	-h

du

#### 4.3处理数据文件

数据排序：sort	-n	-M	-k	-t	-r

数据搜索：grep  [options]  pattern  [file]	-vnce

数据压缩：gzip	gunzip

数据归档：tar  function  [options]  object1  object2..

### 5理解shell

#### 5.1shell的类型

bash csh tcsh dash ...

echo  $0

#### 5.2shell的父子关系

命令列表：一行以" ; "分隔

进程列表：一行以" ; "分隔，整个以" () "包含	生成子shell执行命令（另一种命令分组，" {}; "）

后台模式：command&	将终端与子shell解绑，创建备份文件

协程{

同时做：在后台生成一个子shell，在该子shell中执行命令

coproc command	coproc  name  { command; }

#### 5.3外部命令和内建命令

外部命令：存在于bash shell之外的程序	衍生：耗费时间和资源来设置新子进程的环境

内建命令：与shell编译成一体

type

pwd	两种都有，/usr/bin/pwd

history	!!	!number

别名：alias command='...'	-p



### 6Linux环境变量

#### 6.1什么是环境变量&6.2设置用户自定义变量

环境变量：{

存储shell会话和工作环境的相关信息

引用环境变量  $var

全局变量：{

env	printenv

​	定义：export  局部变量	export  var=value

}

局部变量{

​	定义：my_variable=value	=两边没有空格

}

#### 6.3删除环境变量

删除变量：unset  var

$：用到变量(do anything with)	不加：操作变量(... to)

#### 6.4默认的shell环境变量

#### 6.5设置PATH环境变量

#!/usr/bin/env bash	更好的可移植性

#### 6.6定位系统环境变量

启动bash shell的方式：

default login shell	登录时作为默认登录shell

interactive shell 	作为交互式shell，通过生成子shell启动

non-interactive shell	作为运行脚本的非交互式shell

默认的主启动文件：/etc/profile	执行特定应用程序启动文件[和管理员自定义启动文件]

其余启动文件：提供用户专属的启动文件来定义该用户所用到的环境变量

#### 6.7数组变量

mytest=(zero one two three)

echo ${mytest[index]}	*



### 7理解Linux文件权限

#### 7.1Linux的安全性

Linux安全系统的核心：用户账户

用户权限——>UID——>登录名

/etc/shadow

添加新用户：useradd 	-D	/etc/skel	

删除用户：userdel	-r

修改用户：{

usermod	passwd	chpasswd	

chage	chfn	chsh

}

#### 7.2使用Linux组

唯一的GID和组名

创建新组：groupadd

修改组：groupmod	-g	-n

#### 7.3理解文件权限

rwx	属主  属组  其他用户

设置新建文件和目录的默认权限：umask	umask  <u>number</u>

八进制模式的安全设置	全权限值(full permission)

文件：666	目录：777	权限值=默认权限值-umask

#### 7.4更改安全设置

修改权限：{

chmod  options  mode  file

模式：八进制、符号

符号模式：[ugoa...]   [+-=] [rwxXstugo...]

}

改变所属关系：{

chown	chgrp

​	chown  options  owner [ .group ]  file

​	chgrp  group  file

}

#### 7.5共享文件

3个额外的信息位：SUID、SGID、粘滞位	(八进制位值)

chmod  g+s  dir

#### 7.6访问控制列表(ASL)

*局限性*

ASL：包含多个用户或组的列表和为其分配的权限

setfacl  [options]  

查看ACL：getfacl  file

分配权限：setfacl  [options]  rule  filenames	-m	-x

"+"

ACL继承：d:

### 8管理文件系统

#### 8.1探讨Linux文件系统

ext(extended filesystem)	i节点表

ext2

​	先将数据写入存储设备再更新节点表	易丢失数据

日志文件系统：  临时文件（日志）{

​	日志文件系统方法—模式：数据、有序、回写

​	ext3

​	ext4
​	JFS(journaled  file  system)

​	ReiserFS

​	XFS

}

日志技术：安全性  vs  性能

卷管理文件系统 ：{

写时复制(copy-on-wirte, COW)：快照(snapshot)

*从一个或多个磁盘（或磁盘分区）创建的存储池提供了生成<u>虚拟磁盘</u>（卷）的能力*

​	ZFS—Btrfs—Stratis

}

**存储容量**：kmgt  pezy

#### 8.2使用文件系统

创建分区：{  

工具：

- fdisk
- gdisk
- GNUparted

硬盘分配设备名称：

- SATA驱动器和SCSI驱动器:	/dev/sd<u>x</u>
- SSD NVMe驱动器：	/dev/nvme<u>N</u>n<u>#</u>
- IDE驱动器：	/dev/hd<u>x</u>

}

BIOS(Basic  Input/Output  System)

创建文件系统：mkfs ...

检查和修复文件系统：fsck  options  filesystem

#### 8.3逻辑卷管理

Linux逻辑卷管理器(logical  volume  manager, LVM)：将了一块硬盘上的分区加入已有的文件系统来动态地添加存储空间

LVM布局：{

- 物理卷(physical  volume, PV)：pvcreate
- 卷组(volume  group, VG)：vgcreate
- 逻辑卷(logical volume, LV)：lvcreate

}

LVM：{创建和管理LV的交互式实用工具

​	关键：将额外的存储空间动态添加到LV

首次创建逻辑卷的步骤：

1. 创建物理卷
2. 创建卷组
3. 创建逻辑卷
4. 格式化逻辑卷
5. 挂载逻辑卷

### 9安装软件

#### 9.1软件包管理基础

软件包管理系统	数据库	记录

- Linux系统已安装的软件包
- 每个软件包安装了哪些文件
- 每个已安装的软件包的版本

软件包存储在仓库(repository)服务器

软件包存在依赖关系

软件包管理系统检测工具：

- dpkg

- rpm

  

#### 9.2基于Debian的系统

核心：dpkg命令	用于安装、更新、删除DEB包文件

APT(advanced  package  tool)工具集{

- [ ] apt—cache
- [ ] apt—get
- [ ] apt

apt  [options]  command

}

使用apt管理软件包{

​	apt  --installed  list

​	apt  show  package_name

​	dpkg  -L  package_name

​	dpkg  --search  absolute_file_name



}

使用apt安装软件包{

​	apt  search  package	(--names-only)

​	apt  install  package_name

}

使用apt升级软件：apt  upgrade

使用apt卸载软件包{

​	apt  remove	删除软件包，保留数据和配置文件

​	apt  purge  package_name	全部删除

​	apt  autoremove	删除依赖关系的无用软件包

}

apt仓库{

​	位置：/etc/apt/sources.list

​	指定仓库源：deb(or  deb-src)adress  distribution_name	package_type_list

​	deb：已编译程序的仓库源

​	deb-src：源代码的仓库源

​	address：软件仓库的网址

​	distribution_name：该软件仓库的发行版的版本名称

}

#### 9.3基于Red Hat的系统

前端工具{	基于rpm(命令行工具)

- yum
- zypper
- dnf

}

......

#### 9.4使用容器管理软件

应用程序容器(application  container)：创建环境，包含应用程序运行所需的全部文件，包括运行时库文件。	在任何Linux系统正常运行

容器标准：snap、flatpak

使用snap容器{

1. snap  version
2. snap  list
3. snap  find  app_name
4. snap  info  app_name
5. snap  install  app_name
6. snap remove

}

使用flatpak容器{

1. flatpak  list
2. flatpak  search  app_name
3. flatpak  install  app_ID
4. flatpak  uninstall  app_ID

}

#### 9.5从源代码安装

tarball：源代码发布的软件包形式

### 10文本编辑器

vi编辑器：最早之一，最复杂，大量特性

vim(vi  improved)

检查vim软件包：readlink  -f  path

vim基础{

​	vi  program_name

​	三种操作模式：

​	命令：按键解释为命令，移动光标{	

​		方向键、hjkl

​		G、num G、gg

​	}

​	插入：按“i”进入，“Esc”退出

​	Ex：交互式命令行，命令模式中按“：”{

​	q：未修改数据，退出

​	q!：放弃所有修改，退出

​	w  filename：重命名

​	wq：将缓冲区数据保存到文件，退出

​	}

}

编辑数据{	命令模式

1. 删除：x	dd	dw	d$	J	内容在寄存器
2. 撤销：u
3. 追加：a	A
4. 替换：r	R

}

复制和粘贴{

1. p：从寄存器取回数据
2. 复制：y	yw	y$	可视模式：“v”

}

查找和替换{

​	/*content*	/Enter

​	{	Ex模式	替换命令格式：s/old/new/

​		s/old/new/g：替换当前行所有的

​		n,ms/old/new/g：替换n—m行之间所有的

​		%s/old/new/g：替换所有的

​		%s/old/new/gc：替换所有的，替换时提示

​	}

}



nano、Emacs、KDE、GNOME	......



### 11构建基础脚本

#### 11.1使用多个命令

多个命令同一行，分号分隔	命令行最大字符数255

#### 11.2创建shell脚本文件

文件第一行指定shell：#!/bin/bash

注释：#	不解释#，除了#！

运行脚本{

- 将放置shell脚本文件的目录添加到PATH环境变量中
- 在命令行中使用绝对路径或相对路径来引用shell脚本文件

​	*权限*

}

#### 11.3显示消息

echo	-n



#### 11.4使用变量

环境变量：用$引用

\\$		${var}	{}可选

用户自定义变量{

​	名称：字母、数字、_ 	长度不超过20	区分大小写

​	赋值：“=”两边无空格	var=value	

​	'',"",省略都是字符串

}

命令替换{

​	<u>从命令输出中提取信息并将其赋给变量</u>

​	将命令输出赋给变量：\`command`	$(command)	反引号

}



#### 11.5重定向输入和输出

重定向运算符指向数据流动方向

输出重定向：command   >  outputfile	>>追加

输入重定向{command  <  inputfile

wc：统计数据中的文本

​	内联输入重定向：command  <<  marker	指定文本标记划分起止

}

#### 11.6管道

管道连接(piping){command1  |  command2

同时运行两个命令，在系统内部连接，第一个命令产生输出时，立即传给第二个命令。

串联的数量无限制:command1  |  command2  |  command3

常用：ls  -al  |  more

}

#### 11.7执行数学运算

{	只支持整数运算

expr命令：转义字符(\\)

使用方括号：$[operation]

}

浮点数解决方案{	bash计算器bc	（编程语言）

基本用法

​	能够识别{

- 数字(整数、浮点数)
- 变量(简单变量、数组)
- 注释(#、/**/)
- 表达式
- 编程语句
- 函数

​	scale：控制小数点位数

​			}

在脚本中使用bc{

​	var=$(echo  "option;  expression"  |  bc)

​	var=$(bc  <<  EOF

​	options

​	statements

​	expressions

​	EOF

​	)

​			}

}

#### 11.8退出脚本



查看退出状态码：$?	0~255      

​	保存最后一个已执行命令的退出状态码

exit命令：

#### 11.9实战

纪元时间：date  -d  "Jan  1,  2020"  +%s

### 12结构化命令

结构化命令：根据条件跳过部分命令，改变执行流程

#### 12.1使用if-then语句

格式：

if  command	退出状态码

then			是0执行

​	commands

fi

#### 12.2if-then-else语句

if  command 

then 

​    commands

else

​    commands

fi

#### 12.3嵌套if语句

if  command

then 

​	commands

elif  command

then

​	commands

elif ...

...

else

​	commands

fi

#### 12.4test命令

格式：test  *condition*	condition是一系列参数和值

退出状态码	0：成立	1：不成立

判断3类条件：	整数

- 数值比较：eq	ge	gt	le	lt	ne

- 字符串比较：=	！=	<	>	-n	-z
- 文件比较：-d -e -f -r -s -w -x -O -G filename	-nt -ot

if  [  condition  ]	注意要空格

<，>字符串顺序难题：

1. <，>必须转义，和重定向混淆
2. <，>顺序和sort命令不同

文件比较：shell编程中最为强大且用得最多的比较形式



#### 12.5复合条件测试

布尔逻辑：&&	||	

#### 12.6if-then的高级特性

- 在子shell中执行命令的单括号

- 用于数学表达式的双括号

- 用于高级字符串处理功能的双方括号

  

单括号：(command)		创建子shell

双括号：((  expresion  ))	高级数学表达式，不用转义

双方括号：[[ expression ]]{

​	模式匹配	bashshell

​	==  !=	右侧被视为通配符

​	=~		右侧被视为POSIX扩展正则表达式

}

#### 12.7case命令

列表格式：

case  variable  in

pattern1  |  pattern2)  commands1;;

pattern3)  commands2;;

*)  default  commands;;

esac

#### 12.8实战

### 13更多的结构化命令

#### 13.1for命令

格式：

for  var  in  list

do

​	commands

done



读取列表中的值{	

​	for  v  in  a b c  d	

​	保留var的值	空白字符分隔

​	读取复杂值：转义字符、双引号单引号

​	从变量中读取

​	从命令中读取：$(cat  $file)	file=绝对路径、相对路径或同一个目录下

​	更改字段分隔符：{

​		IFS(internal  field  separator)

​		IFS=$'\n'	IFS.OLD=$IFS

​				}

​	使用通配符读取目录	file   globbing

​		for  file  in  /home/*

}

#### 13.2C语言风格的for命令

格式：

for  ((variable  assignment ; condition ; iteration process))

do

​	commands

done

for  ((a = 1 ; a < 10 ; a++ ))	可空格

#### 13.3while命令

格式：	只要？为0执行，不为0停止

while  test  command

do

​	other  commands

done



使用多个测试命令，只有最后一个决定是否结束



#### 13.4until命令

结束条件和while相反，其他使用相似

格式：	只要？为0停止，不为0执行

until  test  command

do

​	other  commands

done

​	

#### 13.5嵌套循环

#### 13.6循环处理文件数据

综合：嵌套循环，修改IFS环境变量

#### 13.7循环控制

- break命令	n(1)
- continue命令	n(1)

#### 13.8处理循环的输出

管道、重定向

在done命令之后添加处理命令

#### 13.9实战

1. 查找可执行文件
2. 创建多个用户账户{

​	CSV(comma-separated  value, 逗号分隔值)	

​	while  read

}

### 14处理用户输入

#### 14.1传递参数

最基本方法：命令行参数	./test.sh  arg1  arg2 ...

读取参数{

​	位置参数(positional  parameter)接收命令行参数

​		$0-脚本名，$1-第一个命令，...，$9，${10},...

​	引号	'aa  bb'

​	读取脚本名	$0	编写包含多种功能或生成日志消息的工具。拥有一个能识别自己的脚本对于追踪脚本问题、系统审计和生成日志信息非常有用。

​		basename命令

​	参数测试	[ -n  "$1" ]

}

#### 14.2特殊参数变量

参数统计：$#	${!#}

获取所有的数据{

​	$*：一个整体	

​	$@：各个独立单词

}

#### 14.3移动参数

shift命令{	操作命令行参数

​	shift	[n]

​	默认将每个位置的变量值向左移动一个位置	$0不变

}

#### 14.4处理选项

选项：在连字符之后出现的单个字母，能够改变命令的行为

​	长选项：双连字符起始，跟着一个字符串

查找选项{

​	处理简单选项：while、case、shift

​	分离参数和选项：--	同时选用选项和参数

​		while、case、shift、break

​	处理含值的选项：

}

使用getopt命令{

​	接受一系列任意形式的命令行选项和参数，并自动将其转换成适当的格式。能识别命令行参数，简化解析过程。

​	格式：getopt  <u>optstring</u>  parameters

​	在optstring中列出在脚本中要用到的每个字母，在每个需要值的后面加"："

​	在脚本中使用：set  --  $(getopt  -q  ab:cd  "$@")

}

前者在命令行中选项和参数处理后只生成一个输出，后者能够和已有的shell位置变量配合默契

使用getopts命令{

​	每次只处理一个检测到的命令行参数。在处理完所有的参数后，getopts会退出并返回一个大于0的退出状态码。

​	适用在解析命令行参数的循环中

​	格式：getopts  <u>optstring</u>  variable	:	将当前参数保存在命令行中定义的variable	OPTARG,OPTIND

}	在所有shell脚本中使用的全功能命令行选项和参数处理工具

#### 14.5选项标准化

#### 14.6获取用户输入

基本的读取{

​	read	从标准输入(键盘)或另一个文件描述符中接受输入

​	格式：read  variable	-p	REPLY

​	read  -p  "helloworld"  var

​	超时：-t	-n

​	无显示读取：-s

​	从文件中读取：

}

#### 14.7实战

ping命令：快速测试网络主机是否可用

### 15呈现数据

将脚本的输出重定向到Linux系统的不同位置

#### 15.1理解输入和输出

文件描述符{

​	Linux用文件描述符标识每个文件对象

​	非负整数，唯一会标识会话中打开的文件

​	标准文件描述符

文件描述符	缩写	描述

0			STDIN	    标准输入

1			STDOUT	标准输出

2			STDERR	 标准错误

}

STDERR{

​	默认和OUT指向同一个地方

​	重定向错误{

​		只重定向错误：2>  file

​		重定向错误信息和正常输出：

​			1>  file1  2>  file2

​			&>  file    

​			}

}

#### 15.2在脚本中重定向输出

1. 临时重定向每一行

2. 永久重定向脚本中的所有命令

   

临时重定向：>&2	./file 2>  fileERR

​	重定向到文件描述符加&

永久重定向：exec  [0,1,2]>file

#### 15.3在脚本中重定向输入

exec命令将STDIN重定向为文件：exec  0<  file

#### 15.4创建自己的重定向

替代性文件描述符：3~8	

创建输出文件描述符：exec 3>  file	>>追加

重定向文件描述符{

​	恢复已重定向的文件描述符

​	exec  3>&1	

​	exec  1>file

​	exec  1>&3

}

创建输入文件描述符：exec  6<file

创建读/写文件描述符：exec  3<>  file	

​	任何读/写从文件指针上次的位置开始

关闭文件描述符：exec  3>&-

#### 15.5列出打开的文件描述符

lsof命令{	-p	-d

​	列出整个Linux系统打开的所有文件描述符(包括后台进程、登录用户打开的文件)

​	$$：当前进程PID

}

#### 15.6抑制命令输出

将STDERR重定向到null文件

位置：/dev/null

2>  /dev/null

快速清除文件中的数据：cat   /dev/null  >  file

#### 15.7使用临时文件

/tmp{

​	专供临时文件使用的特殊目录，存放不需要永久保留的文件

​	任何用户都有读写权限

​	mktemp

}

创建本地临时文件{

​	指定文件名模板，模板包含任意文本字符，在 末尾加6个X

​	mktemp   testing.XXXXXX	输出为文件名

}

在/tmp命令中创建临时文件：-t      返回完整路径名

创建临时目录：-d

#### 15.8记录消息

tee命令{	连接管道的T型接头

​	将来自STDIN的数据同时送往两处：STDOUT,所指定的文件

​	tee  file	-a

​	配合管道命令：date  |  tee  file

}

#### 15.9实战 

读取CSV格式的数据文件，输出SQL INSERT语句，并将数据插入数据库。

### 16脚本控制

#### 16.1处理信号

Linux系统信号

信号		值		描述

1		SIGHUP	挂起(hang up)进程

2		SIGINT	  中断(interrupt)进程

3		SIGQUIT	停止(stop)进程

9		SIGKILL	  无条件终止(terminate)..

15	      SIGTERM	尽可能终止..

18	      SIGCONT	继续运行停止的..

19	      SIGSTOP	无条件停止，但不终止..

20	      SIGTSTP	停止或暂停(pause)，但不终止..

产生信号

键盘组合键

1. 中断进程：Ctrl+C	SIGINT发送给当前shell所有进程
2. 暂停进程：Ctrl+	SIGTSTP...

​	ps  -l	kill  -9  *PID*

捕获信号{

​	trap命令：指定shell脚本需要侦测并拦截的	信号

​	格式：trap  commands  signals	空格分隔

​	trap  “”  SIGINT

}

捕获脚本退出：trap  "echo bye.."  EXIT

修改或移除信号捕获{

​	修改：重新使用trap命令

​	trap  -p

​	移除：trap  --  SIGINT	trap  -  SIGINT

}

#### 16.2以后台模式运行脚本

ps  -e	看到运行的多个进程

在后台模式中，进程运行时不和终端会话的STDIN、STDOUT、STDERR关联

后台运行脚本{

​	在脚本名后加&

​	./file  &

​	[1]  2563	作业号，PID

​	结束时：[1] +  Done		./file

}

运行多个后台作业

#### 16.3在非控制台下运行脚本

nohup{	让脚本一直以后台模式运行到结束

​	阻断发给特定进程的SIGHUP信号

​	格式：nohup  command

​	将STDOUT、STDERR产生的消息重定向到	nohup.out

​	注意多个命令运行在同目录

}

#### 16.4作业控制

包括:启动、停止、杀死、恢复作业

查看作业{

​	jobs	作业控制中的关键命令，允许用户查看shell当前正在处理的作业

​	+：默认作业，作业控制命令可省略作业号

​	-：下一个默认作业

​	-l	-n  -r  -s

}

重启已停止的作业{	可将已停止的作业作为后台进程或前台进程重启	前台进程会接管当前使用的终端

​	bg命令：以后台模式重启

​	fg命令：以前台模式重启	(foreground)

}

#### 16.5调整谦让度

多任务操作系统中，内核负责为每个运行的进程分配CPU时间

nice  value{	调度优先级、谦让度

​	指内核为进程分配的CPU时间(相对于其他进程)

​	Linux中，由shell启动的所有进程优先级默认为0

​	整数值，-20~+19	-20最高优先级	Nice guys finish last.

}

nice命令：启动命令时设置调度优先级	nice  -n  *number*

renice命令：修改已运行命令的优先级	renice  -n  *number*  -p  *PID*

#### 16.6定时运行作业

在预选时间运行脚本的方法：at命令、cron表、anacron

使用at命令调度作业{

​	将作业提交到队列中，指定Linux系统何时运行脚本

​	atd：at守护进程，在队列中检查待运行的作业

​	格式：at  [-f  filename]  time

时间格式	/usr/share/doc/at/timespec

- 标准小时分钟：10:15
- AM/PM指示符：10:15 PM
- 特殊时间：now、noon、midnight、teatime(4pm)
- 标准日期：MMDDYY、MM/DD/YY、DD.MM.YY
- 文本日期：Jul 4、Dec 25(可加年份)
- 时间增量：
  - Now + 25 minutes
  - 10:15 PM tomorrow
  - 10:15 + 7 days



​	两个队列：a~z、A~Z	-q	(z队列占用较少CPU)

​	获取作业的输出：用户email地址作为STDOUT、STDERR	-M

​	列出等待的作业：atq命令

​	删除作业：atrm  作业号

}

调度需要定期运行的脚本{

​	cron程序调度需要定期运行的作业 	假定Linux系统7*24小时运行

- cron时间表：通过特别的格式指定作业何时运行

​	  格式：minutepasthour hourofday dayofmonth month dayofweek command

​		字段：值、取值范围(28-30)、通配符

​		指定完整路径

- 构建cron时间表：crontab命令	-l	-e
- 浏览cron目录：hourly、daily、monthly、weekly	将脚本复制到相应目录
- anacron：只处理cron目录的程序，保证作业一定能运行

​	时间表格式：period  delay  identifier  command	period以天为单位	

}

#### 16.7使用新的shell启动脚本

#### 16.8实战

辅助需要在运行时避免被中断的脚本

source工具

