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
