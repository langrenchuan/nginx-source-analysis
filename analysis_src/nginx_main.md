# nginx主流程
## 流程图
![](https://github.com/langrenchuan/nginx-source-analysis/blob/master/analysis_src/main.png)
## ngx_debug_init()函数
在设置NGX_DEBUG_MALLOC常量的情况下，打开内存调试，只对FreeBSD系统，Darwin系统生效，其实是改变malloc等内存分配函数，初始化内存地址的方式，会用0xa5来填充，但是会带来性能上的下降，具体参见[manualpage](https://www.freebsd.org/cgi/man.cgi?query=malloc&apropos=0&sektion=0&manpath=FreeBSD+8.0-RELEASE+and+Ports&arch=default&format=html).
## ngx_strerror_init()函数
初始化错误信息。
对于windows这个函数直接返回，后面调用ngx_strerror时再通过windows系统函数获取错误信息。
对类unix系统，strerror()和strerror_r()函数都不是Async-Signal-Safe，因此，它们不能用在信号的处理函数中,因此ngx_strerror_init初始化了一个数组来存放系统错误信息，调用ngx_strerror时对从数组获取错误信息描述来使用。
## ngx_get_options()函数
分析执行参数。
```
Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /usr/local/nginx/)
  -c filename   : set configuration file (default: conf/nginx.conf)
  -g directives : set global directives out of configuration file
```