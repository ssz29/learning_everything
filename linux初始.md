### python3.6.x在Ubuntu16.04下安装过程
```
#(1)保证网络正常连接
# sudo add-apt-repository ppa:jonathonf/python-3.6(服务器403状态,先废弃)
sudo add-apt-repository ppa:deadsnakes/ppa 
sudo apt-get update            (更新软件列表,拿取最新资源)
sudo apt-get install python3.6 (安装python3.6版本)

#(2)调整Python3的优先级，使得3.6优先级较高)
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2

#(3)验证结果: 右键打开终端 -> 输入python3 ->如果出现如下3.6.7版本的提示,证明安装成功,exit()退出
    Python 3.6.12 (default, Aug 18 2020, 02:08:22) 
    [GCC 5.4.0 20160609] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 
```
### linux 跟 windows 的区别         
```
(1)目录的结构
(2)文件格式
	windows 操作系统内核是NT，而linux 是 shell；
	windows 主要文件系统是fat32或NTFS，
	而linux 主要文件系统是Ext2,Ext3
(3)安全性
```
### 目录含义
```
/bin   存放普通用户的命令文件
/boot  存放系统启动文件
/cdrom 存放读取光盘的相关文件
/dev   设备文件 
/etc   配置文件
/home  家目录
/lib   库文件
/lib64 64位库文件
/lost+found 系统异常产生错误时,丢失文件放在这
/media 媒体文件
/mnt   挂载目录
/opt   安装软件时的默认目录
/proc  内存中相关数据文件
/root  root用户登录的家目录
/run   系统运行时候 用到的文件
/sbin  超级管理员运行的文件
/srv   服务启动之后需要访问的数据目录，
/sys   系统文件 
/tmp   临时文件
/usr   应用程序存放目录
/var   放置系统执行过程中经常变化的文件，如随时更改的日志文件 

linux  系统当中 一切皆文件
(常用的比如:普通文件,目录文件,链接文件,设备文件等)
```
### 相关命令
```
相对路径:
.  	  相对于当前路径
.. 	  相对于上一级路径
绝对路径:
以/开头的就是绝对路径

cd .. 回到上一级
cd    切换目录 /home/wangwen
cd ~  切换到家目录
cd -  回到上一个你操作的那一个目录
pwd 看一眼 你当前所在的目录是哪里
ls  看一眼 你这个文件夹里面有什么
.bash_history  但凡是点开头的文件 都是隐藏文件

-a all   所有文件(包括隐藏文件)
-l list  以列表的形式呈现
-h 可以让文件大小带上单位
ll 相当于 ls -al
man是帮助命令 比如:man ls 或 man cp

mkdir 文件夹名称
touch 创建文件
ln -s 创建连接  (ln -s 指定你想要创建的连接  放到哪个目录下面)
ln -s 需要使用绝对路径的方式来创建 
ifconfig 查看linux下面的ip
ln => link s => soft

mv 剪切并且可以改名
	mv source_file(文件) dest_file(文件)
	mv source_file(文件) dest_directory(目录)
cp 复制也可以改名
	# 复制文件夹
    cp -r ceshi300 ceshi400 相对(复制)
	cp -r  ~/ceshi100 ~/ceshi400/ceshi1000000000 绝对(复制并改名)
	cp -a 无论是文件还是文件夹还是权限一并复制过来 

# rm 删除
	rm -rf + 目录
	
	
nano 内置自带的编辑器 编辑文件用的	
cat  查看文件内容
more 对于内容比较多的情况用more 支持分页 空格下一页
head 加上-2 是查看前几条
tail 加上-2 是查看后几条
*vim => sudo apt-get install vim 
```

### 权限结构
```
指定类型(dl-) 权限位1(rwx) 权限位2(rwx) 权限位3(rwx)   r=>read w=>write x=>可执行
d表示文件夹  directory的缩写
l表示链接    link的缩写
-表示文件
d rwx rwx r-x
------ 文件类型-所属主  所属组  其他
  	     d     rwx     rwx    rwx

位数1,2,3 代表当前文件或者文件夹的所有者的权限设定:(所有者的权限  u,user)
		1: r 或者 -  r表示可以读取  - 表示不可以读取
		2: w 或者 -  w表示可以写入  - 表示不可以写入
		3: x 或者 -  x表示可以执行  - 表示不可以执行

位数4,5,6 代表当前文件或者文件夹的所属组的权限设定:(所属组的权限 g ,group)
		1: r 或者 -  r表示可以读取  - 表示不可以读取
		2: w 或者 -  w表示可以写入  - 表示不可以写入
		3: x 或者 -  x表示可以执行  - 表示不可以执行

位数7,8,9 代表其他用户对当前文件或者文件夹的的权限设定:(其他人的权限 o,other)
		1: r 或者 -  r表示可以读取  - 表示不可以读取
		2: w 或者 -  w表示可以写入  - 表示不可以写入
		3: x 或者 -  x表示可以执行  - 表示不可以执行
```

### 更改权限

```
r  => 4
w  => 2
x  => 1

rwx    => 7
rw-    => 6
r-x    => 5
r--    => 4
-wx    => 3
-w-    => 2
--x    => 1
---    => 0
=>rwx的任意组合共 8 种 情况

755 (根目录下默认的文件夹权限)
rwx  r-x  r-x
644 (根目录下默认的文件权限)
rw-  r--  r--
777 (所有权限)
rwx  rwx  rwx

在家目录中,默认创建一个文件的权限是664
在根目录中,默认创建一个文件的权限是644

在家目录中,默认创建一个文件夹的权限是775
在根目录中,默认创建一个文件的权限是755



=>关于权限更改
chmod 755  1.txt
递归更改这个文件夹里面的所有文件权限
chmod -R 777 ceshi100 (chmod -R 777 文件夹)
 
u代表所有者   加一个r权限 (user)
g代表所属组   减一个w权限 (group)
o代表其他	 加一个执行权限 (other)
chmod u+r,g-w,o+x 1.txt

其他写法:
chmod u=rwx 1.txt

=>对于目录来讲  
r   是否呈现里面的文件
w   是否可以在里面创建文件或文件夹
x   cd 切不进来  不能访问这个目录

=>对于文件来讲
r  可以看到文件内容
w  可以更改删除文件及内容
x  是否可以执行这个文件 ./abc.sh (shell) 


(*扩展)
*sudo useradd a01  添加用户
*sudo passwd a01   给用户添加密码
*sudo su a01       切换用户
*exit  	           退出当前用户  
sudo userdel a01   删除用户a01

```


### 查找命令
```
1 find	 搜索命令	  #找文件		
	
	按照文件名查找
	find  查找位置   -name  文件名
	find  /  -name  index.php		按照文件名查找
			-iname			       按照文件名查找，不区分大小写


2 grep 	"字符串"  文件名		 #找这个文件里面符合条件的内容(找内容)

	  -v		反向选择
	  -i 		忽略大小写
	
	grep  -i  "root"  /etc/passwd
	grep  -v  "root"  /etc/passwd

*(ls -l 路径 /ss/xx/文件 ) 单独查看一个文件或文件夹的
/etc
passwd 账户文件
shadow 账户的密码文件(加密的)
root 是最高权限账户

```
### 关于挂载操作
    --查看当前系统有哪些挂载设备
    sudo fdisk -l  
    
    /dev/sda1  *        2048 37750783 37748736  18G 83 Linux    (主分区)
    /dev/sda2       37752830 41940991  4188162   2G  5 扩展     (扩展分区) (需要划分逻辑分区才能使用)
    /dev/sda5       37752832 41940991  4188160   2G 82 Linux 交换 / Solari  (交换分区)
    oom(out of memory) 内存溢出死机 linux不会出现,因为交换分区的存在,可以临时作为内存空间进行调度,如果负载过大,默认杀死后台占空间资源最大的进程,以节省资源
    
    --挂载
    sudo mount 找到的设备路径  /mnt/cdrom
    --取消挂载
    sudo umount /mnt/cdrom (umount + 挂载的目录)
