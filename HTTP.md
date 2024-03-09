# HTTP协议

超文本传输协议, HyperText Transfer Protocol.

## 请求报文格式

```bash
<method> <url> <version> CRLF
<headers> CRLF

<body>
```

### method

#### GET

查

#### POST

增

#### PUT

改

#### DELETE

删

#### TRACE

测试, 诊断.

#### OPTIONS

请求查询服务器的性能, 或者查询与资源相关的选项和需求.

#### CONNECT

保留.

### url

请求资源地址(不包括域名地址).

### version

版本号, 分为1.0(短连接), 1.1(长连接), 2.0(完全兼容1.1).

#### 1.1的新功能(仅列出已经使用过的功能)

1. 增加了Host请求头, 使得可以在同个端口上搭建多个虚拟WEB站点.
2. 将请求头Connection的默认值设置为Keep-Alive, 如果要关闭连接复用需要显式的设置Connection:Close.

### Headers

请求头, 在HTTP/1.1 协议中, 所有的请求头, 除Host外, 都是可选的.

### body

请求主体, GET无请求主体, 可以传送任意数据块, 例如: name=XXX&pwd=XXXX, xml文件, html文件.

#### PS

CRLF是换行符

## 响应报文格式

```bash
<version> <status-code> <status-text> CRLF
<headers> CRLF

<body>
```

### status-code

状态码.

#### 1xx

指示信息. 表示请求已接收, 继续处理.

#### 2xx

成功. 表示请求已被成功接收, 理解, 接受.

#### 3xx

重定向. 要完成请求必须进行更进一步的操作.

#### 4xx

客户端错误. 请求有语法错误或请求无法实现.

#### 5xx

服务器端错误. 服务器未能实现合法的请求.

#### 详细状态码解释

```txt
100——客户必须继续发出请求
101——客户要求服务器根据请求转换HTTP协议版本

200——交易成功
201——提示知道新文件的URL
202——接受和处理、但处理未完成
203——返回信息不确定或不完整
204——请求收到，但返回信息为空
205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
206——服务器已经完成了部分用户的GET请求

300——请求的资源可在多处得到
301——删除请求数据
302——在其他地址发现了请求数据
303——建议客户访问其他URL或访问方式
304——客户端已经执行了GET，但文件未变化
305——请求的资源必须从服务器指定的地址得到
306——前一版本HTTP中使用的代码，现行版本中不再使用
307——申明请求的资源临时性删除

400——错误请求，如语法错误
401——请求授权失败
402——保留有效ChargeTo头响应
403——请求不允许
404——没有发现文件、查询或URl
405——用户在Request-Line字段定义的方法不允许
406——根据用户发送的Accept拖，请求资源不可访问
407——类似401，用户必须首先在代理服务器上得到授权
408——客户端没有在用户指定的饿时间内完成请求
409——对当前资源状态，请求不能完成
410——服务器上不再有此资源且无进一步的参考地址
411——服务器拒绝用户定义的Content-Length属性请求
412——一个或多个请求头字段在当前请求中错误
413——请求的资源大于服务器允许的大小
414——请求的资源URL长于服务器允许的长度
415——请求资源不支持请求项目格式
416——请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段
417——服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求

500——服务器产生内部错误
501——服务器不支持请求的函数
502——服务器暂时不可用，有时是为了防止发生系统过载
503——服务器过载或暂停维修
504——关口过载，服务器使用另一个关口或服务来响应用户，等待时间设定值较长
505——服务器不支持或拒绝支请求头中指定的HTTP版本
```

### status-text

状态码的文本描述.

### body

响应主体.