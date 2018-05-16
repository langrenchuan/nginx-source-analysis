#Nginx源码分析过程中遇到的问题
##configure文件疑惑
###test命令以及，shell里面的||是什么意思
|| && 其实就是或与例如  

	echo 1 || echo 2 
	1
	echo 1 && echo 2
	1
	2

test命令条件判断命令例如
	
	src是一个目录则用-d返回true用-f返回false
	test -d src || echo 1
	test -f src || echo 1
	1
	

###shell里option
可以轻松获取脚本执行选项例如

test.sh 脚本如下

	for  option
	do
   		echo $option
	done
	
执行后可输出
	
	$ ./test.sh op1 op2
	op1
	op2

###Make命令是如何工作的
无论是在linux 还是在Unix环境 中，make都是一个非常重要的编译命令。不管是自己进行项目开发还是安装应用软件，我们都经常要用到make或make install。_利用make工具，我们可以将大型的开发项目分解成为多个更易于管理的模块，对于一个包括几百个源文件的应用程序，使用make和 makefile工具就可以简洁明快地理顺各个源文件之间纷繁复杂的相互关系。_

而且如此多的源文件，如果每次都要键入gcc命令进行编译的话，那对程序员 来说简直就是一场灾难。而make工具则可自动完成编译工作，并且可以只对程序员在上次编译后修改过的部分进行编译。

因此，有效的利用make和 makefile工具可以大大提高项目开发的效率。同时掌握make和makefile之后，您也不会再面对着Linux下的应用软件手足无措了。
关于make命令的工作原理这篇文章比较详细http://www.ruanyifeng.com/blog/2015/02/make.html ，官方文档纯英文https://www.gnu.org/software/make/manual/make.html。
###.开头的命令
.开头的命令将后面的文件变为当前shell执行，但是必须是有一层目录才行。
	
	. dir/your.sh
	
###