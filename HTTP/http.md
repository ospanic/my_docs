## 预备知识
要想理解HTTP协议，需要先具备以下基础知识，包括 ：IP、域名、DNS、TCP、UDP。下面先对这些知识做一些简单的介绍。

### IP(仅介绍IPv4)
IP地址是用来在网络中标记一台网络设备的一串数字，一个IP地址由4个字节组成，比如 ```192.168.1.1``` 就是一个简单的IP地址。要想访问网络中的某个设备，必须事先知道要访问设备的IP地址。事实上，IP地址是非常难以记忆的，于是乎，人们就发明域名，使用域名与IP地址一一对应，这样就将难以记忆的IP地址，转换成了便于记忆的域名。

IP地址被划分为A、B、C、D、E五类，有兴趣额的同学可参考下[IP地址的分类]()。

### 域名
一个域名可以由有字母(不区分大小写)、数字、“.”、“-”、“_”组成，```www.taobao.com```就是一个常见的域名。要想获得域名，需要在域名提供商处购买，国内 阿里云、腾讯云、百度云等等都提供域名注册服务，有兴趣的同学可以去买一个玩玩。域名最常用的功能是将网址转换成难以记忆的IP，但域名的功能不仅限于此，有兴趣的同学可以参考一下[域名解析类型详解]()。

### DNS(域名服务器)
域名服务器中记录了域名与IP地址的对应关系，比如我们要访问莫个网站时，不需要知道这个网站的IP地址，只需要知道这个网站的域名，然后在DNS服务器上查找域名对应的IP地址，就可以访问该网站了。

有兴趣的同学可以学习一下[DNS协议]()。

### 网站/网址
如果让你说出一个你最熟悉的网站，你可能会脱口而出：百度，那么百度这个网站的网址是什么呢？大部分人应该都能回答上来，百度的网址是```www.baidu.com``` 。没么，百度的网站是如何与百度的网址绑定到一起的呢？实际上，百度的网站部署在网络中的一台服务器上，可以通过指令 ```ping www.baidu.com``` 查看百度网站服务器的IP地址。在DNS服务器中，```www.baidu.com```这个域名与百度服务器的IP地址绑定在一起，所以我们就能够通过网址```www.baidu.com```，来访问百度的网站了。

### 端口号
现实生活中，我们要想给某个人写信，一定要先知道收信人的地址。在网络通信中也一样，要想与网络中的某台设备通信，就要事先知道目标设备的IP地址。但是只知道设备的IP地址往往是不够的，就如同一个小区中有很多住户，只知道小区的名字是无法把信件准确地送到收件人手中的。一个网络设备中可能运行了很多个软件，只知道设备的IP地址，同样无法精确地把信息传送给特定的软件。于是就引入了端口的概念，端口的范围是0-65535。设备中的某个软件想要使用网络通信，就必须占据一个或多个端口。比如192.168.1.1设备上的SSH软件占据并监听22端口，那么如果我们向192.168.1.1的22端口发送数据，192.168.1.1设备上的SSH就能收到我们发送的数据。

在现代的计算机系统中，一些比较靠前的端口后都已经被一些软件默认占据了，有兴趣的同学请参考
[默认端口号及通信方式]()。

### TCP通信
知道某个设备的IP地址及某个软件的端口号，就可以与该软件通信了，常用通信协议有两种，一种是TCP，另一种是UDP。

TCP协议是一种有链接的协议，即通开始信前要先建立TCP连接，通信完成后需断开TCP连接。TCP协议保证数据能够准确地送达接收方。 关于TCP连接的过程，请参考[TCP连接三字握手四次挥手]()。

HTTP使用的就是TCP通信，默认端口是80。

### UDP通信
UDP通信是一种无连接的通信方式，通信前无需建立连接，通信后也无需断开连接。由于无连接的特性，UDP通信不保证数据能够准确地送达接收方。

DNS使用的是UDP通信，默认端口是53。

## HTTP协议
在浏览器的地址栏里输入一个网址并按下回车键后，浏览器会先向DNS服务器查找网址中域名对应的IP，找到IP地址后，与该IP的相关端口(默认80)建立TCP连接，然后按照HTTP协议进行数据交互。具体的交互过程可分为浏览器发起请求，服务器响应请求。

本文章重在讨论HTTP协议，关于浏览器如何通过DNS服务器查找域名对应的IP，有兴趣的同学可以参考[DNS协议]()。

上面介绍完了基础知识，下面就要介绍HTTP协议了，需要使用到的工具有浏览器，[网络调试工具](http://mdocs.oss-cn-shenzhen.aliyuncs.com/http/NetAssist.exe)。下文将会使用网络调试工具模拟HTTP请求及HTTP响应。

补充一个特殊的IP地址：```127.0.0.1```，这个IP地址指向正在使用的电脑本身，也就是自己用来访问自己的地址，常常用于在一台机器上调试某个网络应用程序。接下来的介绍中将会频繁使用到这个地址。

### HTTP请求
> 本实验使用网络调试工具模拟HTTP服务器，查看浏览器发来的请求内容。

如下图所示，打开网络调试助手。
- 协议类型选择  ```TCP Server```
- 本地主机地址选择 ```127.0.0.1```
- 本地主机端口输入 ```80```

然后点击链接按钮

![TCP Server 配置](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http1.png)

打开浏览器，在地址栏中输入```127.0.0.1```，按下回车键，网络调试助手中将会收到类似(不同的浏览器略有不同)下图的数据：

![TCP Server 收到数据](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http2.png)

收到的数据便是HTTP的请求头，下面我们将详细分析下HTTP请求头：

    GET / HTTP/1.1
    Host: 127.0.0.1
    Connection: keep-alive
    Upgrade-Insecure-Requests: 1
    User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
    Accept-Encoding: gzip, deflate, br
    Accept-Language: zh-CN,zh;q=0.9
    (空行)
    (空行)


**首先我们来看第一行，前三个字母```GET```表示HTTP的请求类型为GET请求，空格后面的```/```表示请求路径为根目录，再一个空格后面的```HTTP/1.1```表示使用的HTTP版本为1.1版本。**

如果我们在浏览器中输入```127.0.0.1/hello```, 那么请求头的第一行将会如下，有兴趣的同学可自行尝试。

    GET /hello HTTP/1.1

**第二行表示我们访问的主机名称为```127.0.0.1```, 如果是通过域名来访问某个网站，Host这里将会是相应的域名。**

如果我们在浏览器中输入```www.baidu.com/nihao```, 那么请求头的前两行将会如下所示：

    GET /nihao HTTP/1.1
    Host: www.baidu.com
> 这里我们补充一点Host字段的作用。考虑如下情况，在一台服务器上建立的两个或更多的网站，这些网站使用的都是80端口。比如小明购买了一台云服务器，并在上面建立了两个网站分别是```www.xiaoming.com```和```blog.xiaoming.com```，那么我们访问这两个网站事实上访问的是同一台服务器，服务器是如何知道我们要访问的是哪个网站呢？聪明的同学已经猜到了，服务器正是通过这个Host字段知道的，我们访问```www.xiaoming.com```这个网站时请求头中的Host字段为```www.xiaoming.com```，访问```blog.xiaoming.com```这个网站时请求头中的Host字段为```blog.xiaoming.com```。

**第三行 ```Connection``` 含义是连接类型，有如下两种**

- ```keep-alive``` 告诉服务器请求完成后保持连接，等待下次请求。
- ```close``` 告诉服务器请求完成后断开连接。

**第四行暂不讨论**

**第五行 ```User-Agent``` 为用户标识**

```User-Agent``` 中会包含浏览器及操作系统信息。

**结尾两个空行**

第一个空行表示请求头结束，下面的内容是请求体。可以理解为第一个空行用于分割请求头和请求体。

第二个空行表示请求内容，由于请求内容为空，所以就只有一个空行。

**HTTP请求结构及常用字段总结**

| 字段  | 含义 | 是否必选 |示例|
|------|------|---------|------|
|首行|请求类型、请求路径、协议版本| 必选|GET /hello HTTP/1.1
|Host|主机名称|几乎必选|Host: www.baidu.com|
|Connection| 连接类型| 可选|Connection: keep-alive|
|User-Agent|浏览器标识|可选|User-Agent: Mozilla/5.0|
|Accept|可接受的数据类型|可选|Accept: text/html|
|Accept-Encoding|可接受的编码方式|可选|Accept-Encoding: gzip|
|Accept-Language|可接受的语言|可选|Accept-Language: zh-CN|
|空行|标志请求头结束，接下来是请求内容|必选||
|请求内容||可选||
|空行|标志请求内容结束| 必选|

>注意: 字段与数据之间由一个冒号和一个空格间隔，如```Host: www.baidu.com```,中间有一个冒号和一个空格。

### HTTP响应

> 上面的实验使用网络调试工具模拟HTTP服务器，分析了浏览器发来的请求内容。接下来我们将使用网络调试工具来模拟浏览器，分析一下HTTP服务器响应数据的格式。

我们以```www.baidu.com```这个网站做实验，首先我们要获得这个网站的IP地址。在命令行中输入 ```ping www.baidu.com```获得其IP地址如下：```14.215.177.38```

    C:\Users\ZhangPeng>ping www.baidu.com

    正在 Ping www.a.shifen.com [14.215.177.38] 具有 32 字节的数据:
    来自 14.215.177.38 的回复: 字节=32 时间=5ms TTL=56
    来自 14.215.177.38 的回复: 字节=32 时间=6ms TTL=56
    来自 14.215.177.38 的回复: 字节=32 时间=6ms TTL=56
    来自 14.215.177.38 的回复: 字节=32 时间=6ms TTL=56

    14.215.177.38 的 Ping 统计信息:
        数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，
    往返行程的估计时间(以毫秒为单位):
        最短 = 5ms，最长 = 6ms，平均 = 5ms

打开网络调试助手，按如下设置，然后点解连接按钮
- 协议类型选择 ```TCP Client```
- 本地主机地址 ```选择下拉框中的第一个```
- 远程主机地址 ```14.215.177.38``` 刚刚获得的百度的IP地址

![TCP Client 配置](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http3.png)

连接成功后发送如下数据：

    GET / HTTP/1.1
    Host: www.baidu.com
    (空行)
    (空行)

如果一切正常将受到如下图所示数据：

![TCP Client 收到数据](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http4.png)

下面我们分析一下收到的数据：

    HTTP/1.1 200 OK
    Accept-Ranges: bytes
    Cache-Control: no-cache
    Connection: Keep-Alive
    Content-Length: 14615
    Content-Type: text/html
    Date: Sat, 06 Apr 2019 09:42:03 GMT
    Etag: "5c9c7bd5-3917"
    Last-Modified: Thu, 28 Mar 2019 07:46:29 GMT
    P3p: CP=" OTI DSP COR IVA OUR IND COM "
    Pragma: no-cache
    Server: BWS/1.1
    Set-Cookie: BAIDUID=F2E6F53AB606C170571A43D3B17161F0:FG=1; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com
    Set-Cookie: BIDUPSID=F2E6F53AB606C170571A43D3B17161F0; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com
    Set-Cookie: PSTM=1554543723; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com
    Vary: Accept-Encoding
    X-Ua-Compatible: IE=Edge,chrome=1

    <!DOCTYPE html><!--STATUS OK-->
    <html>
    <head>
        <meta http-equiv="content-type" content="text/html;charset=utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=Edge">
    (内容过多，省略一部分......)
    </body></html>
    (空行)

**首先我们来看第一行**

```HTTP/1.1```表示协议版本，空格后面的```200```表示响应状态代码，再一个空格后面的```OK``表示响应状态。

**接下来我们来看第四行**

```Content-Length: 14615```表示响应内容(不包括响应头)的长度为14615个字节

**最后我们分析下响应内容**

在收到的相应数据中，前面的部分是响应头，一个空行之后将是响应内容，最后再以一个空行结束。响应内容可以说就是这两个空行之间的数据，其长度应该与响应头中的```Content-Length```值一致。本次实验的响应内容是百度首页的源代码。

**HTTP响应结构及常用字段总结**

| 字段  | 含义 | 是否必选 |示例|
|------|------|---------|------|
|首行|协议版本、响应状态吗、响应状态| 必选|HTTP/1.1 200 OK|
|Accept-Ranges|服务器所支持的内容范围|可选|Host: www.baidu.com|
|Connection| 连接类型| 可选|Connection: keep-alive|
|Content-Length|响应内容的长度(不包括响应头)|几乎必选|Content-Length: 348|
|Content-Type|响应内容的类型|可选|Content-Type: text/html|
|空行|标志响应头结束，接下来是响应内容|必选||
|响应内容||可选||
|空行|标志响应内容结束| 必选|

### 模拟HTTP响应

> 上面已经介绍了HTTP的请求格式和HTTP的响应格式，接下来我们将使用网络调试工具模拟一个HTTP服务器，通过浏览器访问该服务器，网络调试工具响应浏览器的访问请求。

打开网络调试助手，配置如下图：

![TCP Server 配置](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http1.png)

打开浏览器，在地址栏输入```127.0.0.1```，按下回车键，网络调试助手将收到浏览器发来的请求，我们向浏览器回应如下数据：

    HTTP/1.1 200 OK
    Content-Length: 12

    Hello World!
    (空行)

![TCP Server 配置](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http5.png)

**如果一切正常，浏览器将显示如下内容：**

![TCP Server 配置](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http6.png)

本次实验通过网络调试工具按照HTTP响应格式响应了浏览器的访问请求，响应内容为```Hello World!```, 响应内容长度为12。浏览器解析了我们返回的数据，并将响应内容显示了出来，说明我们返回给浏览器的数据格式是正确的。

## 总结

通过这篇文章，相信大家对HTTP有了一定的了解，实际上HTTP只是基于TCP的一种应用协议，HTTP服务器的默认使用端口是80。HTTP请求过程总结如下：

- 客户端与HTTP服务器建立TCP连接
- 客户机通过刚才建立的TCP连接发送HTTP请求数据给HTTP服务器，最简单的请求示例如下：
>
    GET / HTTP/1.1
    Host: www.baidu.com
    (空行)
    (空行)

- HTTP服务器通过TCP连接发送HTTP响应数据给客户机，最简单的响应示例如下：
> 
    HTTP/1.1 200 OK
    Content-Length: 12

    Hello World!
    (空行)

- 根据情况决定关闭TCP连接还是保持连接继续发起请求


HTTP默认使用80端口，事实上HTTP可以使用任意未被占用的端口。我们使用网络调试助手建立TCP Server，本地IP设为```127.0.0.1```，端口设为```81```，在浏览器中输入```127.0.0.1:81```,网络调试助手同样能收到浏览器发来的的HTTP请求。日常生活中我们很少输入端口号，是因为大多说网站都使用默认的80端口，我们不输入端口，浏览器就默认我们输入的是是80端口，所以我们也能正常访问网站。如果非要指定端口，在地址栏输入```www.baidu.com:80```,也能正常访问百度。

实际上HTTP请求类型有很多种，本文只介绍了最简单常用的GET请求，有兴趣的同学可以自己学习。HTTP请求类型有:
- ```GET```：向特定的资源发出请求。 
- ```HEAD```：向服务器索要与GET请求相一致的响应，只不过响应体将不会被返回。这一方法可以在不必传输整个响应内容的情况下，就可以获取包含在响应消息头中的元信息。 
- ```POST```：向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的创建和/或已有资源的修改。 
- ```PUT```：向指定资源位置上传其最新内容。 
- ```DELETE```：请求服务器删除Request-URI所标识的资源。 
- ```OPTIONS```：返回服务器针对特定资源所支持的HTTP请求方法。也可以利用向Web服务器发送'*'的请求来测试服务器的功能性。 
- ```TRACE```：回显服务器收到的请求，主要用于测试或诊断。 
- ```CONNECT```：HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。

HTTP的请求头、响应头的字段也非常多，本文也只是介绍了几个常用的字段，有兴趣的同学可以参考[常用的HTTP请求头与响应头](https://www.cnblogs.com/honghong87/p/6941436.html)
