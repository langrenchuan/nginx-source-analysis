#`auto`
##综述
configure文件，所用到的大部分脚本都在auto下面，阅读源码可以从configure文件来阅读，逐步认识nginx的编译过程。
##options
configure里率先引入auto/options脚本来执行初始化nginx编译过程中的绝大部分环境变量，并且根据执行configure是的选项来重新定义这些环境变量。
##init
auto/init文件，初始化了一些文件或声明文件地址，包括
	


| 文件 | 作用 | 初始化 |
| --- | :-----: | --: |
|Makefile | Makefile 文件描述了整个工程的编译、连接等规则| 创建
|ngx_modules.c | 默认被包含进系统中的模块的声明,如果想去掉一些模块，只要修改这个文件即可| 定义
|ngx\_auto_headers.h|关于系统头文件存在性的声明|定义|
|autoconf.err|configure过程中的一些信息|定义|
|ngx\_auto_config.h|根据环境检查的结果声明的一些宏定义,这个头文件被include进 ngx_core.h中，所以会被所有的源码引用到，这确保了源码是可移植的|定义|

##sources
auto/sources 初始化一些核心源码的环境变量。
##cc/
定义不同的编译器，以及编译器的相关参数
##headers
GNU下的header引入
##include
通过编译生成可执行文件方法来确定系统库的引入。
##feature
此工具的核心思想是，输出一小段代表性c程序，然后设置好编译选项，再进行编译连接运行，再对结果进行分析。例如，如果想检测某个库是否存在，就在小段c程序里面调用库里面的某个函数，再进行编译链接，如果出错，则表示库的环境不正常，如果编译成功，且运行正常，则库的环境检测正常。
##os/
平台相关的一些系统参数与系统调用相关的检测,跨平台性主要在这里体现。

##threads
线程池相关配置，windows不支持线程池.

##have
	
	cat << END >> $NGX_AUTO_CONFIG_H

	#ifndef $have
	#define $have  1
	#endif

	END

从代码中，我们可以看到，这个工具的作用是，将$have变量的值，宏定义为1，并输出到auto_config文件中。通常我们通过这个工具来控制是否打开某个特性。这个工具在使用前，需要先定义宏的名称 ，即$have变量
##lib/
对nginx编译所需要的基础库的检测
##make
生成makefile文件
##install
生成与安装相关的makefile文件内容，并生成最外层的makefile文件
##stub
ngx_auto_config.h中加入NGX_SUPPRESS_WARN、NGX_SMP宏,后面分析源码的时候再看看是什么意思。
##summary
总结输出信息