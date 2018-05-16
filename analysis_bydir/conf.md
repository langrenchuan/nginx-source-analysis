Conf
conf目录是，在make install的时候将要拷贝到对应目录下的配置相关内容。
# nginx.conf
nginx配置的唯一入口，其他文件如果有需要才会include进来，nginx配置语法后面将花大幅篇幅讲解。
# koi-win
	charset_map  koi8-r  windows-1251 {
	     80  88 ;  euro
	     ...
	}
	
定义koi8-r字符集到windows-1251字符集的map,完整的http支持的字符集见http://www.iana.org/assignments/character-sets/character-sets.xhtml 
# koi-utf
	charset_map  koi8-r  utf-8 {

   		80  E282AC ;  euro
	}
	
定义koi8-r字符集到utf-8字符集的map.

# win-utf
	charset_map  windows-1251  utf-8 {

  		82  E2809A ;  single low-9 quot
  	}

定义windows-1251字符集到utf-8字符集的map.
# fastcgi.conf fastcgi_params
	fastcgi_param  SCRIPT_FILENAME    		$document_root$fastcgi_script_name;
 	fastcgi_param  QUERY_STRING       $query_string;
 	fastcgi_param  REQUEST_METHOD     $request_method;
 	fastcgi_param  CONTENT_TYPE       $content_type;
 	fastcgi_param  CONTENT_LENGTH     $content_length;
 	...
定义fastcgi的一些变量.
# mime.types
定义response header里Content-Type: 的map
# scgi_param
定义scgi的一些参数
# uwsgi_param
与uWSGI服务器进行交换的一些参数。WSGI是一种Web服务器网关接口
# 各个类型cgi的含义
## CGI = Common Gateway Interface
顾名思义，它是一种接口规范。该规范详细定义了Web服务器中运行的服务器代理程序，怎样获取及返回网页生成过程中，服务器环境上下文和HTTP协议中的参数名称，如大家所熟知的：REQUEST_METHOD，QUERY_STRING，CONTENT_TYPE等等。绝大部分的Web服务器程序，是以脚本的形式代理接受并处理HTTP请求，返回HTTP页面或响应。这些脚本程序，就是大家所熟知的PHP、ASP、JSP等等。

## FCGI = Fast CGI
它其实是CGI在具体实现中的的一个变种。其设计思路是，通过减少CGI代理程序和Web宿主服务程序的通信开销，从而达到提高Web服务性能的最终目的。由此可见，FCGI在规范上跟CGI并没有不同，只是具体实现方式上有所改进：CGI的做法是，对于每个HTTP请求，Web宿主服务程序都建立新的进程以调用服务器脚本，相应该请求；FCGI的做法是，建立一个独立的FCGI服务程序进程，和Web宿主服务程序进程通信，FCGI服务进程被一旦启动后，自己分配资源、创建线程响应HTTP请求、并决定自身生命周期，从而大大降低了系统为了创建进程而做出的资源开销。现代流行的Web服务器程序，如PHP、ASP.Net，基本都是FCGI的实现。

## SCGI = Simple CGI
它是FCGI在精简数据协议和响应过程后的产物。其设计目的是为了适应越来越多基于AJAX或REST的HTTP请求，而做出更快更简洁的应答。并且SCGI约定，当服务器返回对一个HTTP协议请求响应后，立刻关闭该HTTP连接。所以不难看出，SCGI更加适合于普遍意义上SOA所提倡的“请求-忘记”这种通信模式。

## WSGI = Web Server Gateway Interface
此协议是Python语言的专利，它定义了一组在Web服务宿主程序和HTTP响应代理程序之间通信的普遍适用的接口。它的产生是因为Python程序员注意到，对于Web框架和Web宿主服务器程序间，有严重的耦合性，比如说，某些框架是针对Apache的mod_python设计的。于是，WSGI就定义了一套非常低级别的接口。常见的Python Web框架都实现了这个协议：如 CherryPy, Django, web.py, web2py, TurboGears, Tornado, Pylons, BlueBream, Google App Engine[dubious – discuss], Trac, Flask, Pyramid,等等.
