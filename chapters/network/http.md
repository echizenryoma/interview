## HTTP

### HTTP协议

HTTP（超文本传输协议，HyperText Transfer Protocol）是互联网上应用最为广泛的一种网络协议。所有的WWW文件都必须遵守这个标准。设计HTTP最初的目的是为了提供一种发布和接收HTML页面的方法。是用于从WWW服务器传输超文本到本地浏览器的传输协议。默认使用80端口，HTTP客户端发起一个请求，建立一个到服务器指定端口（默认是80端口）的TCP连接。HTTP协议和TCP协议是不冲突的，HTTP定义在七层协议中的应用层，TCP解决的是传输层的逻辑。HTTP使用TCP而不是UDP的原因在于（打开）一个网页必须传送很多数据，而TCP协议提供传输控制，按顺序组织数据，和错误纠正。HTTP协议的瓶颈及其优化技巧都是基于TCP协议本身的特性。如TCP建立连接时三次握手有1.5个RTT（round-trip time）的延迟，为了避免每次请求的都经历握手带来的延迟，应用层会选择不同策略的HTTP长链接方案。又如TCP在建立连接的初期有慢启动（slow start）的特性，所以连接的重用总是比新建连接性能要好。

### 请求报文

HTTP 协议是以 ASCII 码传输，建立在 TCP/IP 协议之上的应用层规范。规范把 HTTP 请求分为三个部分：状态行、请求头、消息主体。

```
<method> <request-URL> <version>
<headers>

<entity-body>
```

**GET请求报文示例**：

```
GET /books/?sex=man&name=Professional HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6) Gecko/20050225 Firefox/1.0.1
Connection: Keep-Alive
```

**POST请求报文示例**：

```
POST / HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
Gecko/20050225 Firefox/1.0.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 40
Connection: Keep-Alive

sex=man&name=Professional
```

### 页面加载

1. 解析域名
2. 建立TCP连接
3. 发送HTTP请求
4. 服务器处理请求并返回HTTP报文
5. 浏览器解析渲染页面
6. 连接结束

### HTTP/1.1相较于HTTP/1.0的区别

* HTTP/1.1在继承了HTTP/1.0优点的基础上，也克服了HTTP/1.0的性能问题。
* HTTP/1.1支持长链接，在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和
* 关闭连接的消耗和延迟，客户端和服务端都是默认对方支持长链接的。
* HTTP/1.1允许客户端不用等待上一次请求结果返回，就可以发出下一次请求。
* 在HTTP/1.1，Request和Response头中都有可能出现一个Connection的头，此Header的含义是当客户端和服务端通信时对于长链接如何进行处理。
* HTTP/1.1通过增加更多的请求头和响应头来改进和扩充HTTP/1.0的功能。HTTP/1.1还提供了与身份认证、状态管理和Cache缓存等机制相关的请求头和响应头。例如：_HTTP/1.0不支持Host请求头字段，WEB浏览器无法使用主机头名来明确表示要访问服务器上的哪个WEB站点，这样就无法使用WEB服务器在同一个IP地址和端口号上配置多个虚拟WEB站点。 _
* HTTP/1.1支持文件断点续传。

### 请求方式

* `GET`请求获取Request-URI所标识的资源
* `POST`在Request-URI所标识的资源后附加新的数据
* `HEAD`请求获取由Request-URI所标识的资源的响应消息报头
* `PUT`请求服务器存储一个资源，并用Request-URI作为其标识
* `DELETE`请求服务器删除Request-URI所标识的资源
* `TRACE`请求服务器回送收到的请求信息，主要用于测试或诊断
* `CONNECT`保留将来使用
* `OPTIONS`请求查询服务器的性能，或者查询与资源相关的选项和需求

### 状态代码

* 1xx：指示信息——表示请求已接收，继续处理。
* 2xx：成功——表示请求已被成功接收、理解、接受。
* 3xx：重定向——要完成请求必须进行更进一步的操作。
* 4xx：客户端错误——请求有语法错误或请求无法实现。
* 5xx：服务器端错误——服务器未能实现合法的请求。

#### 常见的状态码

* `200 OK` 一切正常，对GET和POST请求的应答文档跟在后面。
* `204 No Content` 没有新文档，浏览器应该继续显示原来的文档。如果用户定期地刷新页面，而Servlet可以确定用户文档足够新，这个状态代码是很有用的。
* `301 Moved Permanently` 客户请求的文档在其他地方，新的URL在Location头中给出，浏览器应该自动地访问新的URL。
* `302 Found` 类似于301，但新的URL应该被视为临时性的替代，而不是永久性的。
* `400 Bad Request` 请求出现语法错误。
* `401 Unauthorized` 客户试图未经授权访问受密码保护的页面。应答中会包含一个WWW-Authenticate头，浏览器据此显示用户名字/密码对话框，然后在填写合适的Authorization头后再次发出请求。
* `403 Forbidden` 资源不可用。服务器理解客户的请求，但拒绝处理它。通常由于服务器上文件或目录的权限设置导致。
* `404 Not Found` 无法找到指定位置的资源。这也是一个常用的应答。
* `502 Bad Gateway` 服务器作为网关或者代理时，为了完成请求访问下一个服务器，但该服务器返回了非法的应答。

### HTTP幂等性

* `GET`用于获取资源，不应有副作用，所以是幂等的。
* `DELETE`用于删除资源，有副作用，但它应该满足幂等性。
* `POST`用于创建资源，所对应的URI并非创建的资源本身，而是去执行创建动作的操作者，有副作用，不满足幂等性。
* `PUT`用于创建或更新操作，所对应的URI是要创建或更新的资源本身，有副作用，它应该满足幂等性。

### 缓存

#### 首部

##### 通用首部

| **字段** | **说明** |
| :---: | :---: |
| Cache-Control | 控制缓存行为 |
| Pragma | `no-cache`禁用缓存（HTTP/1.0） |

##### 请求首部

| **字段** | **说明** |
| :---: | :---: |
| If-Match | 比较与`ETag`字段是否一致 |
| If-None-Match| 比较与`ETag`字段是否不一致 |
| If-Modified-Since | 比较最后修改时间是否一致 |
| If-Unmodified-Since | 比较最后修改时间是否不一致 |

##### 响应首部

| **字段** | **说明** |
| :---: | :---: |
| ETag | 匹配信息 |

##### 实体首部

| **字段** | **说明** |
| :---: | :---: |
| Expires | 实体过期时间（HTTP/1.0） |
| Last-Modified | 最后修改时间 |

### 跨站请求伪造\(Cross-site request forgery, CSRF\)

1. 检查Referer字段
2. 添加校验Token

### 跨站脚本\(Cross-site scripting, XSS\)

1. 过滤特殊字符
2. 使用HTTP头指定类型

### HTTPS

HTTPS = HTTP + SSL/TLS

#### 优缺点

HTTPS在传输数据之前需要客户端与服务器进行一个握手\(SSL/TLS握手\)，在握手过程中将确立双方加密传输数据的密码信息。TLS/SSL使用了非对称加密，对称加密以及信息摘要函数等。

HTTPS相比于HTTP，虽然提供了安全保证，但是势必会带来一些时间上的损耗，如握手和加密等过程，是否使用HTTPS需要根据具体情况在安全和性能方面做出权衡。

