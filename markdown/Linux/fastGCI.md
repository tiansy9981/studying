# 前言

因为服务器的项目使用到了fastCGI，所以来学习。

# 一、CGI

在说fastCGI之前，我们要先了解下CGI。毕竟fastCGI是对CGI的优化

## CGI介绍

通用网关接口（Common Gateway Interface/CGI）描述了客户端和服务器程序之间传输数据的一种标准，可以
让一个客户端，从网页浏览器向执行在网络服务器上的程序请求数据。CGI 独立于任何语言的，CGI 程序可以用任何脚本语言或者是完全独立编程语言实现，只要这个语言可以在这个系统上运行。（其实就是对web服务器解析的http请求数据进行处理的）

## CGI工作流程

在了解CGI是如何工作之前，我们应该首先知道，整个客户端和服务器请求与响应的处理过程，明白CGI在其中担任什么角色。
整个过程如下图所示：

![在这里插入图片描述](https://img-blog.csdnimg.cn/0c1c32484b914c5fa6b9ae8245e0838c.png)

具体流程如下：
（1）用户通过浏览器访问服务器, 发送了一个请求, 请求的url如下：http://localhost/login?user=zhang3&passwd=123456&age=12&sex=man
（2）服务器接收数据, 对接收的数据进行解析
（3） nginx对于一些登录数据不知道如何处理, nginx将数据发送给了cgi程序，服务器端会创建一个cgi进程
（4）CGI进程执行
（5）服务器将cgi处理结果发送给客户端

了解整体的工作过程，我们知道了其实CGI就是web服务器解析的http请求数据进行处理的。接下来我们把CGI工作的具体流程展开来看。
CGI内部具体结构如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/8eeffac730a0402983473be89167f651.png)

CGI进程执行具体过程：
（1）加载配置, 如果有需求加载配置文件获取数据
（2）连接其他服务器: 比如数据库
（3）逻辑处理
（4）得到结果, 将结果发送给服务器
（5）退出

![在这里插入图片描述](https://img-blog.csdnimg.cn/c0ece4ad3b3940608f7bada66143060f.png)

## CGI的弊端

从上面整个流程我们知道，nginx将数据发送给cgi程序时，服务器端会创建一个cgi进程。
可见，每当需要处理数据时，都需要频繁的创建和销毁进程，服务器开销太多，效率太低。
因此，我们接下来可以看看fastCGI

# 二、fastCGI

## 介绍

快速通用网关接口（Fast Common Gateway Interface／FastCGI）是通用网关接口（CGI）的改进，描述了客户端和服务器程序之间传输数据的一种标准。 FastCGI致力于减少Web服务器与 CGI 程式 之间互动的开销，从而使
服务器 可以同时处理更多的Web请求 。与为每个请求创建一个新的进程不同，FastCGI使用持续的进程来处理
一连串的请求。这些进程由FastCGI进程管理器管理，而不是web服务器。（nginx下fastcgi与服务器是分离的）

## fastCGI与CGI的区别

CGI 就是所谓的短生存期应用程序，FastCGI 就是所谓的长生存期应用程序。FastCGI像是一个常驻(long-live)型的CGI，它可以一直执行着，不会每次都要花费时间去fork一次
普通CGI就是每发布一个请求，就创建一个进程。
FastCGI是常驻进程，常驻服务，从下面一行代码便看看出

```bash
while (FCGI_Accept() >= 0)
```

**fastCGI工作流程**
工作流程如下：（注意看fastCGI中的流程与CGI的区别）

![在这里插入图片描述](https://img-blog.csdnimg.cn/69c4a9201b15454fb83c94db91d82dbc.png)

具体流程如下：
（1）用户通过浏览器访问服务器, 发送了一个请求, 请求的url如下
http://localhost/login?user=zhang3&passwd=123456&age=12&sex=man
（2）服务器接收数据, 对接收的数据进行解析
（3）nginx对于一些登录数据不知道如何处理, nginx将数据发送给了fastcgi程序
通过本地套接字，网络通信的方式 （通过socket接口进行连通）
（4）启动fastCGI （不是由web服务器直接启动，而是通过一个fastCGI进程管理器启动）
加载配置 - 可选；连接服务器 - 数据库；循环（服务器有请求 -> 处理，将处理结果发送给服务器，没有请求 -> 阻塞）
（5）服务器将fastCGI的处理结果发送给客户端

了解了fastCGI后，接下来我们就开始要学习针对它的使用了

# 三、fastCGI的使用

## nginx下的请求

nginx 不能像apache那样直接执行外部可执行程序，但nginx可以作为代理服务器，将请求转发给后端服务器，
这也是nginx的主要作用之一。其中nginx就支持FastCGI代理，接收客户端的请求，然后将请求转发给后端fastcgi进程。下面介绍如何使用C/C++编写cgi/fastcgi，并部署到nginx中。通过前面的介绍知道，fastcgi进程由FastCGI进程管理器管理，而不是nginx。这样就需要一个FastCGI管理，管理我们编写fastcgi程序。我们使用spawn-fcgi作为FastCGI进程管理器。spawn-fcgi是一个通用的FastCGI进程管理器，简单小巧，原先是属于lighttpd的一部分，后来由于使用比较广泛，所以就迁移出来作为独立项目了。spawn-fcgi使用pre-fork 模型， 功能主要是打开监听端口，绑定地址，然后fork-and-exec创建我们编写的fastcgi应用程序进程，退出完成工作 。fastcgi应用程序初始化，然后进入死循环侦听socket的连接请求。

## 流程介绍

依旧，先上流程图，让我们有直观的认识

![在这里插入图片描述](https://img-blog.csdnimg.cn/27dd156d86334e90a88dad5974c0d7aa.png)

具体步骤如下：
（1）客户端访问, 发送请求，请求的url如下
http://localhost/login?user=zhang3&passwd=123456&age=12&sex=man
（2）nginx web服务器, 无法处理用户提交的数据
（3）spawn-fcgi - 通信过程中的服务器角色
被动接收数据，在spawn-fcgi启动的时候给其绑定IP和端口
（4）执行fastCGI程序
程序猿写的 -> login.c -> 可执行程序( login )；使用 spawn-fcgi 进程管理器启动 login 程序, 得到一进程

nginx.conf文件的配置
通过请求的url http://localhost/login?user=zhang3&passwd=123456&age=12&sex=man 转换为一个
指令:
去掉协议（http）
去掉域名/IP + 端口
如果尾部有文件名 去掉
去掉 ? + 后边的字符串
剩下的就是服务器要处理的指令: /login
进行相应配置如下：

```yaml
location /login
{
	# 转发这个数据, fastCGI进程
	fastcgi_pass 地址信息:端口;
	# fastcgi.conf 和nginx.conf在同一级目录: /usr/local/nginx/conf
	# 这个文件中定义了一些http通信的时候用到环境变量, nginx赋值的
	include fastcgi.conf;
}
#地址信息:
	- localhost
	- 127.0.0.1
	- 192.168.1.100
	#端口: 找一个空闲的没有被占用的端口即可
```


启动spawn-fcgi

```
# 前提条件: 程序猿的fastCGI程序已经编写完毕 -> 可执行文件 login
spawn-fcgi -a IP地址 -p 端口 -f fastcgi可执行程序
- IP地址: 应该和nginx的 fastcgi_pass 配置项对应
- nginx: localhost -> IP: 127.0.0.1
- nginx: 127.0.0.1 -> IP: 127.0.0.1
- nginx: 192.168.1.100 -> IP: 192.168.1.100
- 端口:
应该和nginx的 fastcgi_pass 中的端口一致
```


**fastCGI程序编写**
通过对fastCGI的源码安装目录下的例子（fastcgi/example下的echo.c文件）来学习使用fastCGI。
对这个例子代码的分析：

![在这里插入图片描述](https://img-blog.csdnimg.cn/bb5625cd6bc440f8b60bb1c9be4c8705.png)

在我们实现自己逻辑代码的时候，依照下面的流程即可：

```
// http://localhost/login?user=zhang3&passwd=123456&age=12&sex=man
// 要包含的头文件
#include "fcgi_config.h" // 可选
#include "fcgi_stdio.h" // 必须的, 编译的时候找不到这个头文件, find->path , gcc -I
// 编写代码的流程
int main()
{
	// FCGI_Accept()是一个阻塞函数, nginx给fastcgi程序发送数据的时候解除阻塞
	while (FCGI_Accept() >= 0)
	{
		// 1. 接收数据
		// 1.1 get方式提交数据 - 数据在请求行的第二部分
		// user=zhang3&passwd=123456&age=12&sex=man
		char *text = getenv("QUERY_STRING");
		// 1.2 post方式提交数据
		char *contentLength = getenv("CONTENT_LENGTH");
		// 根据长度大小判断是否需要循环
		// 2. 按照业务流程进行处理
		// 3. 将处理结果发送给nginx
		// 数据回发的时候, 需要告诉nginx处理结果的格式 - 假设是html格式
		printf("Content-type: text/html\r\n");
		printf("<html>处理结果</html>");
	}
}
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/6f0715dc80594836a5f3c5f9c4711d51.png)

