# gcc-4.8.5
gcc-4.8.5 dev-envs


下载GCC源码以及所有依赖包
分成服务器联网和断网两种情况

# 联网
执行命令行 

    1.wget http://ftp.gnu.org/gnu/gcc/gcc-4.8.5/gcc-4.8.5.tar.gz
    2.tar –zxvf gcc-4.8.5.tar.gz
    3.cd gcc-4.8.5
    4. ./contrib/download_prerequisites //执行脚本自动下载、安装依赖包

# 断网

   wget http://ftp.gnu.org/gnu/gcc/gcc-4.8.5/gcc-4.8.5.tar.gz
   wget ftp://gcc.gnu.org/pub/gcc/infrastructure/mpfr-2.4.2.tar.bz2
   wget ftp://gcc.gnu.org/pub/gcc/infrastructure/gmp-4.3.2.tar.bz2
   wget ftp://gcc.gnu.org/pub/gcc/infrastructure/mpc-0.8.1.tar.gz

手动下载gcc-4.8.5.tar.gz、gmp-4.3.2.tar.gz、mpfr-2.4.2.tar.gz、mpc-0.8.1.tar.gz上传到用户

	tar –zxvf gcc-4.8.5.tar.gz
	cd gcc-4.8.5  

将gmp-4.3.2.tar.gz、mpfr-2.4.2.tar.gz、mpc-0.8.1.tar.gz移入gcc-4.8.5
通过查看cat ./contrib/download_prerequisites 脚本的动作 手动解压、安
装依赖包

1. tar -xjf mpfr-2.4.2.tar.gz         //解压  
	ln -sf mpfr-2.4.2  mpfr      //建立软连接  
2. tar -xjf gmp-4.3.2.tar.gz  
	ln –sf gmp-4.3.2 gmp 
3. tar -xzf mpc-0.8.1.tar.gz  
	ln –sf mpc-0.8.1  mpc

生成Makefile 编译新的gcc、安装
	cd gcc-4.8.5
	mkdir objdir  
	cd objdir
	../configure --disable-checking --enable-languages=c,c++ --disable-multilib --prefix=/unlimited/install/gcc-4.8 --enable-threads=posix
	make //执行makefile  （可用多线程 过程漫长 大约3GB保证磁盘空间够用  可用df 查看）
	make install  //安装 （不那么漫长 300MB   du -sh * 可查看当前文件大小）

/unlimited/install/gcc-4.8 是一个当前用户不受限制的绝对路径
例如: /home/username/myinstall/gcc-4.8 依葫芦画瓢也可将其
他应用安装myinstall下面。

配置环境变量
环境配置有好几种，这边不多哔哔赖赖，可以看看这里：
https://blog.csdn.net/byxdaz/article/details/88857720

编辑 ~/.bashrc 文件

		vim ~/.bashrc
		export PATH= /home/username/myinstall/gcc-4.8/bin: /home/username/myinstall/gcc-4.8/lib64$PATH  
		export LD_LIBRARY_PATH= /home/username/myinstall/gcc-4.8/lib64: /home/username/myinstall/gcc-4.8/lib$LD_LIBRARY_PATH
    
		source ~/.bashrc  //更新该文件

到这算是成功了，
用 gcc -v 查看当前用户版本


用 ldd +可执行文件 查看

# 环境变量
PATH    可执行程序的查找路径

查看当前环境变量:

echo $PATH

设置:

方法一：export PATH=PATH:/XXX 但是退出当前终端后就失效

方法二：修改 ~/.bashrc 或 ~/.bash_profile或系统级别的/etc/profile

1. 在其中添加例如export PATH=/opt/ActivePython/bin:$PATH

2. source .bashrc  (Source命令也称为“点命令”，也就是一个点符号（.）。

source命令通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录)

LD_LIBRARY_PATH   动态库的查找路径

我们在ubuntu图形界面下用eclipse写了一个动态库，到centos下调用时出现错误，   

error while loading shared libraries: libmysqlclientso.so.0: cannot open shared object file: No such file or directory
以为没装mysql-client，因为ubuntu下叫这个，但是centos下直接就叫mysql，服务器版本叫mysql-server，查找了一下libmysqlclient.so

find / -name libmysqlclient.so，果然发现不同：

这是因为没有把动态链接库的安装路径（例如说是 /usr/local/lib ）放到变量LD_LIBRARY_PATH里。

这时，可以用命令 export 来临时测试确认是不是这个问题：

命令行：export LD_LIBRARY_PATH=./lib:$LD_LIBRARY_PATH

或者     export LD_LIBRARY_PATH=/home/other/test/lib:$LD_LIBRARY_PATH    

? 配置之后最好运行一下：ldconfig

在终端里运行上面这行命令，再运行这个可执行文件，如果运行正常就说明是这个问题。

接下来的问题是：以上做法，只是临时设置变量 LD_LIBRARY_PATH ，下次开机，一切设置将不复存在；如何把这个值持续写到 LD_LIBRARY_PATH 里呢？

我们可以在 ~/.bashrc 或者 ~/.bash_profile 中加入 export 语句，前者在每次登陆和每次打开 shell 都读取一次，后者只在登陆时读取一次。我的习惯是加到 ~/.bashrc 中，在该文件的未尾，可采用如下语句来使设置生效：

export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
修改完后，记得关掉当前终端并重新打开一个新的终端，从而使上面的配置生效。也可以不关闭终端再打开终端，使用source ~/.bashrc或source ~/.bash_profile使上面的配置生效。

[source：使当前shell读入路径为filepath的shell文件并依次执行文件中的所有语句，通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录]

不过这个环境变量是基于shell的，也就是说只有在当前设置了的shell里才会有作用，因此每次打开新的shell运行程序不停地设置LD_LIBRARY_PATH，

是一件非常麻烦的事情，所以我们就会想有不有什么一劳永逸地方法，使得设置之后就不用再去设置了？答案是肯定的，

即在~/目录下打开.bash_profile文件，设置环境变量如下：

LD_LIBRARY_PATH=dir：$LD_LIBRARY_PATH
export LD_LIBRARY_PATH
LD_LIBRARY_PATH  这个环境变量是大家最为熟悉的，它告诉loader：在哪些目录中可以找到共享库。可以设置多个搜索目录，这些目录之间用冒号分隔开。

在linux下，还 提供了另外一种方式来完成同样的功能，你可以把这些目录加到/etc/ld.so.conf中，然后调用ldconfig。

当然，这是系统范围内全局有效 的，而环境变量只对当前shell有效。按照惯例，除非你用上述方式指明，

loader是不会在当前目录下去找共享库的，正如shell不会在当前目前找 可执行文件一样。

