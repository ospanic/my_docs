# 如何在网页内发起HTTP请求

## 预备知识
读懂此篇文章你需要先了解 ```JavaScript``` 和 ```html``` 基础语法。

## 一个简单的网页

    <html>  
        <head>
            <title>Http Dome</title>
            <script stype="text/javascript">

                function led_on()
                {
                    var x = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHttp")
                    x.onreadystatechange = function()
                    {
                        if (x.readyState == 4 && x.status == 200){}
                    };

                    x.open("GET", "/ON", true);
                    x.send();
                }
    
            </script>
        </head>

        <body>
            <div style="text-align: center" >
                <button  onclick="led_on(); return false;">LED_ON(开灯)</button>
                <button  onclick="save();return false;">LED_OFF(关灯)</button>
            </div>
        </body>
    </html>

点击网页上的按钮，将会发起一个HTTP请求，请求的路径是```/ON```,下面我们将通过实验查看一下请求过程

## HTTP实验

工具：浏览器 、 [网络调试助手]()

### 建立HTTP服务器

打开网络调试助手，按照如下图所示配置建立HTTP服务器

![建立HTTP服务器](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http1.png)

### 浏览器访问刚才建立的服务器

打开浏览器，输入127.0.0.1，按回车键，网络调试助手将会收到如下数据：

![建立HTTP服务器](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http2.png)

### 回复数据给浏览器
粘贴如下代码到网络调试助手的发送数据输入框，点击发送按钮

    HTTP/1.1 200 OK
    Content-Length: 778

    <html>  
        <head>
            <title>Http Dome</title>
            <script stype="text/javascript">

                function led_on()
                {
                    var x = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHttp")
                    x.onreadystatechange = function()
                    {
                        if (x.readyState == 4 && x.status == 200){}
                    };

                    x.open("GET", "/ON", true);
                    x.send();
                }
    
            </script>
        </head>

        <body>
            <div style="text-align: center" >
                <button  onclick="led_on(); return false;">LED_ON(开灯)</button>
                <button  onclick="save();return false;">LED_OFF(关灯)</button>
            </div>
        </body>
    </html>

![回复数据给浏览器](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http_in1.png)

**浏览器将显示如下图所示的网页**

![浏览器网页显示](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http_in2.png)

**清空网络调试助手中接收的数据，点击浏览器上的LED_ON按钮，网络调试助手将会收到类似如下数据**

![浏览器网页显示](https://mdocs.oss-cn-shenzhen.aliyuncs.com/http/http_in3.png)

## 原理分析
在上问的试验中，我们通过在网页内点击按钮的方式，发起了一个HTTP请求。

在JavaScript代码中，我们创建了一个```led_on()```函数，并将次函数绑定到了按键LED_ON的点击事件上。当点击LED_ON按钮时就会执行```led_on()```函数。该函数向```/ON```路径发送了一个http get 请求，所以网络调试助手就收到了如上数据。