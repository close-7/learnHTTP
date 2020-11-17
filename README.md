# learnHTTP
HTTP协议学习-协议就是一种规则
## 了解HTTP协议
### 输入网址后发生了什么
+ 输入网址，发往dns服务器进行域名查询（先检查本地host文件，没有找本地dns服务器）
+ dns服务器根据域名解析返回ip地址
+ 浏览器根据IP地址，向web服务器发送请求
+ web服务器根据请求，回传页面内容

### HTTP协议
+ 超文本传输协议（HTTP）是一种通信协议，它允许将超文本标记语言（HTML）文档从web服务器传送到客户端的浏览器
+ HTTP是一个属于应用层的面向对象的协议
+ HTTP协议是构建在TCP/IP协议之上，是TCP/IP协议的一个子集

### web与HTTP
+ WEB是基于超文本和HTTP的，全球性，动态交互的，跨平台的分布式图形信息系统
+ 建立在Internet上的一种网络服务，为浏览者在internet上查找和浏览信息提供图形化，易于访问的直观界面

### TCP/IP协议族
+ TCP/IP协议其实是一系列与互联网想关联的协议结合起来的总称
+ 分层管理是TCP/IP协议的重要特征
    1. 应用层 ：应用层一般是我们编写的应用程序，决定了想用户提供的应用服务。应用层可以通过系统调用与传输层进行通信。如：FTP,DNS,HTTP等
    2. 传输层 ：传输层通过系统调用向应用层提供处于网络连接中的两台计算机之间的数据传输功能。如TCP和UDP。
    3. 网络层 -- 用来处理在网络上流动的数据包，数据包是网络传输的最小数据单位。该层规定了通过怎样的路径（传输路线）到达对方计算机，并把数据包传输给对方（IP）
    4. 链路层 -- 用来处理连接网络的硬件部分，包括控制操作系统，硬件设备驱动，NIC（Network Interface Card，网络适配器）以及光纤等物理可见部分。

### 数据包的封装过程
![数据包的封装过程](./img/1.png "数据包的封装过程")

### HTTP数据传输过程
![HTTP数据传输过程](./img/2.png "HTTP数据传输过程")

### 传输层-TCP三次握手--确认双的收发能力
![传输层-TCP三次握手](./img/3.png "传输层-TCP三次握手")
+ 第一次握手： 客户端发送带有SYN标志的连接请求报文段，然后客户端进入SYN_SEND状态，等待服务端的确认
+ 第二次握手： 服务端接收到客户端的SYN报文段后，需要发送ACK信息对这个SYN报文段进行确认。同时，还要发送自己的SYN请求信息。服务端会将上述的信息放到一个报文段（SYN+ACK报文段）中，一并发送给客户端，此时服务端将会进入SYN_RECV状态。
+ 第三次握手： 客户端接收到服务端的SYN+ACK报文段后，会向服务端发送ACK确认报文段，这个报文段发送完毕后，客户端和服务端都进入ESTABLISHED状态，完成TCP三次握手。

### DNS域名解析
DNS：提供域名到IP地址之间的解析服务，DNS服务也与HTTP协议密不可分。

### 抓取分析http请求的工具
[wireshark](https://www.wireshark.org/)


## HTTP协议结构和通信原理

### HTTP协议特点
+ 支持客户/服务器模式
    1. 客户/服务器模式工作的的方式是由客户端向服务器发送请求，服务器端响应请求，并进行相应服务
+ 简单快速：
    1. 客户发送请求服务时，只需要传送请求方法（GET,POST,HEAD）和路径
    2. 由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快
+ 灵活：
    1. HTTP允许传输任意类型的数据对象
    2. 正在传输的类型由于Content-Type（Content-Type是HTTP包中用来表示内容类型的标识）加以标记
+ 无连接： 
    1. 无连接的含义是限制每次连接只处理一个请求
    2. 服务器处理完客户的请求，并收到客户的应答后，即断开连接
+ 无状态：
    1. 无状态是指协议对于事物处理没有记忆功能。缺少状态意味着如果后续需要处理前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大

### URI和URL
+ URI：可以分为URL,URN或同时具备locators和names特性的一个东西
+ URN作用就好像一个人的名字，URL就像一个人的地址
+ URN是唯一标识的一部分是身份信息，URL提供了找到它的方式
+ URI和URL最大的差别是“访问机制”

### HTTP报文结构分析-请求报文
![请求报文](./img/5.png "请求报文")

+ HTTP报文头 HTTP/1.1里一共规范了47种报文头字段：
    1. HTTP报文头大体分为四类，
        + 通用报文头
            1. Cache-Control	指定请求和响应遵循的缓存机制	Cache-Control: no-cache
            2. Connection	表示是否需要持久连接。（HTTP 1.1默认进行持久连接）	Connection: close
            3. Date	请求发送的日期和时间	Date: Tue, 15 Nov 2010 08:12:31 GMT
            4. Pragma	用来包含实现特定的指令	Pragma: no-cache
            5. Trailer	指出头域在分块传输编码的尾部存在	Trailer: Max-Forwards
            6. Transfer-Encoding	报文主体的传输编码方式	Transfer-Encoding:chunked
            7. Upgrade	向服务器指定某种传输协议以便服务器进行转换（如果支持）	Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11
            8. Via	通知中间网关或代理服务器地址，通信协议	Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1)
            9.Warning	关于消息实体的警告信息	Warn: 199 Miscellaneous warning
        + 请求报文头
            1. Accept	指定客户端能够接收的内容类型	Accept: text/plain, text/html
            2.Accept-Charset	浏览器优先接受的字符编码集。	Accept-Charset: iso-8859-5
            3. Accept-Encoding	指定浏览器可以支持的web服务器返回内容压缩编码类型。	Accept-Encoding: compress, gzip
            4. Accept-Language	浏览器优先的语言	Accept-Language: en,zh
            5. Authorization	HTTP授权的授权证书	Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==
            6. Expect	请求的特定的服务器行为	Expect: 100-continue
            7. From	发出请求的用户的Email	From: user@email.com
            8. Host	指定请求的服务器的域名和端口号	Host: www.zcmhi.com
            9. If-Match	只有请求内容与实体相匹配才有效	If-Match: “737060cd8c284d8af7ad3082f209582d”
            10. If-Modified-Since	如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回304代码	If-Modified-Since: Sat, 29 Oct 2010 19:43:31 GMT
            11. If-None-Match	如果内容未改变返回304代码，参数为服务器先前发送的Etag，与服务器回应的Etag比较判断是否改变	If-None-Match: “737060cd8c284d8af7ad3082f209582d”
            12. If-Range	如果实体未改变，服务器发送客户端丢失的部分，否则发送整个实体。参数也为Etag	If-Range: “737060cd8c284d8af7ad3082f209582d”
            13. If-Unmodified-Since	只在实体在指定时间之后未被修改才请求成功	If-Unmodified-Since: Sat, 29 Oct 2010 19:43:31 GMT
            14. Max-Forwards	限制信息通过代理和网关传送的时间	Max-Forwards: 10
            15. Proxy-Authorization	连接到代理的授权证书	Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==
            16. Range	只请求实体的一部分，指定范围	Range: bytes=500-999
            17. Referer	先前网页的地址，当前请求网页紧随其后,即来路	Referer: http://www.zcmhi.com/archives/71.html
            18. TE	客户端愿意接受的传输编码，并通知服务器接受接受尾加头信息	TE: trailers,deflate;q=0.5
            19. User-Agent	User-Agent的内容包含发出请求的用户信息	User-Agent: Mozilla/5.0 (Linux; X11)
        + 响应报文头
            1. Accept-Ranges	表明服务器是否支持指定范围请求及哪种类型的分段请求	Accept-Ranges: bytes
            2. Age	从原始服务器到代理缓存形成的估算时间（以秒计，非负）	Age: 12
            3. ETag	请求变量的实体标签的当前值	ETag: “737060cd8c284d8af7ad3082f209582d”
            4. Location	用来重定向接收方到非请求URL的位置来完成请求或标识新的资源	Location: http://www.zcmhi.com/archives/94.html
            5. Proxy-Authenticate	它指出认证方案和可应用到代理的该URL上的参数	Proxy-Authenticate: Basic
            6. Retry-After	如果实体暂时不可取，通知客户端在指定时间之后再次尝试	Retry-After: 120
            7. Server	web服务器软件名称	Server: Apache/1.3.27 (Unix) (Red-Hat/Linux)
            8. Vary	告诉下游代理是使用缓存响应还是从原始服务器请求	Vary: *
            9. WWW-Authenticate	表明客户端请求实体应该使用的授权方案	WWW-Authenticate: Basic
        + 实体报文头
            1. Allow	对某网络资源的有效的请求行为，不允许则返回405	Allow: GET, HEAD
            2. Content-Encoding	web服务器支持的返回内容压缩编码类型。	Content-Encoding: gzip
            3. Content-Language	响应体的语言	Content-Language: en,zh
            4. Content-Length	响应体的长度	Content-Length: 348
            5. Content-Location	请求资源可替代的备用的另一地址	Content-Location: /index.htm
            6. Content-MD5	返回资源的MD5校验值	Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==
            7. Content-Range	在整个返回体中本部分的字节位置	Content-Range: bytes 21010-47021/47022
            8. Content-Type	返回内容的MIME类型	Content-Type: text/html; charset=utf-8
            9. Expires	响应过期的日期和时间	Expires: Thu, 01 Dec 2010 16:00:00 GMT
            10. Last-Modified	请求资源的最后修改时间	Last-Modified: Tue, 15 Nov 2010 12:45:26 GMT

### HTTP报文结构分析-响应报文
![响应报文](./img/6.png "响应报文")

### HTTP请求方法
1. GET
    + 用来请求访问已被URI识别的资源
    + 指定的资源经服务器端解析后返回响应内容
    + 可以用来提交表单和其他数据：/1.html?id=1&page=2
    + 浏览器对于提交URL长度有所限制
2. POST
    + 一般用来传输实体的主体
3. PUT
    + 从客户端向服务器传送数据取代指定的文档的内容
    + 与POST不同：PUT是幂等的，而POST是不幂等的
    + PUT方法更多用作传输资源
4. HEAD
    + 类似GET，只不过返回的响应中没有具体的内容，用于获取报头
5. DELETE
    + 请求服务器删除指定的资源
6. OPTIONS
    + 用来 查询 针对请求URI指定资源支持的方法
7. TRACE
    + 回显服务器收到的请求，主要用于测试或诊断
8. CONNECT
    + 开启一个客户端与所请求资源之间的双向沟通的通道，它可以用来创建隧道

### HTTP响应状态码
>是用以表示网页服务器超文本传输协议响应状态的3位数字代码
+ 1xx 信息，服务器收到请求，需要请求者继续执行操作
+ 2xx 成功，操作被成功接收并处理
    1. 200	OK	请求成功。一般用于GET与POST请求
    2. 202	Accepted	已接受。已经接受请求，但未处理完成
    3. 206	Partial Content	部分内容。服务器成功处理了部分GET请求
+ 3xx 重定向，需要进一步的操作以完成请求
    1. 301	Moved Permanently	永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替
    2. 302	Found	临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI
+ 4xx 客户端错误，请求包含语法错误或无法完成请求
    1. 400	Bad Request	客户端请求的语法错误，服务器无法理解
    2. 401	Unauthorized	请求要求用户的身份认证
    3. 403	Forbidden	服务器理解请求客户端的请求，但是拒绝执行此请求
    4. 404	Not Found	服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面
+ 5xx 服务器错误，服务器在处理请求的过程中发生了错误
    1. 500	Internal Server Error	服务器内部错误，无法完成请求
    2. 502	Bad Gateway	作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应


### 用telnet分析http协议的通讯过程

### HTTP状态管理： Cookie与Session
+ Cookie
    1. Cookie实际上是一小段文本信息。客户端请求服务器，如果需要记录该用户状态，就向客户端颁发一个Cookie
    2. 客户端会把Cookie保存。当客户端再请求该网站时，浏览器把请求的网站连同Cookie一同提交给服务器。服务器检查Cookie，辨认用户状态

+ Session 
    1. Session是另一种记录客户状态的机制，保存在服务器上。客户端浏览器访问服务器的时候，服务器会把客户端信息以某种形式记录在服务器上
    2. 客户端再次访问时只需要从该Session中查找该客户的状态就可以了
    3. 保存Session ID的方式
        + Cookie
        + URL重写
        + 隐藏表单
    4. Session有超时失效
    5. 程序调用HttpSession.invalidate()主动失效
    6. 服务器进程被停止
+ 异同：
    1. 存放位置不同
    2. 安全性不同
    3. 有效期上的不同
    4. 对服务器压力的不同

## HTTP协议特性和使用方法

### HTTP协议之基本认证
+ BASIC认证（基本认证）
+ DIGEST（摘要认证）
+ SSL（客户端认证）
+ FormBase 认证（基于表单认证）

### HTTP的长连接和短连接
>HTTP的长连接和短连接本质上是TCP长连接和短连接
>HTTP/1.1起，默认使用长连接，用以保持连接特性

### 代理的作用
+ 抓包
+ fq
+ 匿名访问
+ 过滤器

### HTTP缓存头部字段
+ Cache-Control 
    1. 请求/响应头，缓存控制字段
    2. 可选值：
        + no-store:所有内容都不缓存
        + no-cache：缓存，但是浏览器使用缓存钱，都会请求服务器判断缓存资源是否是最新
        + max-age=x(秒) 请求缓存后X秒不再发起请求
        + s-maxage=x(秒) 代理服务器请求源站缓存后的X秒不再发起请求，只对CDN缓存有效
        + public 客户端和代理服务器CDN都可以缓存
        + private 只有客户端可以缓存
+ Expires
    1. 响应头，代表资源过期时间，由服务器返回提供，是http1.0的属性，在于max-age共存的情况下，优先级要低
+ Last-Modified
    1. 响应头，资源最新修改时间，由服务器告诉浏览器
+ if-Modifies-Since
    1. 请求头， 资源最新修改时间，由浏览器告诉服务器，和Last-Modified是一对，它两会进行对比
+ Etag
    1. 响应头，资源标识，由服务器告诉浏览器
+ if-None-Match
    1. 请求头，缓存资源标识，由浏览器告诉服务器(其实就是上次服务器给的Etag)，和Etag是一对，它两会进行对比

### HTTP缓存工作方式
1. 让服务器与浏览器约定一个文件过期时间---Expires
2. 让服务器与浏览器在约定文件过期时间的基础上，再加一个文件最新修改时间的对比---Last-Modified和if-Modifies-Since
3. 让服务器与浏览器在过期时间Expires+Last-Modified的基础上，增加一个文件内容唯一对比标记---Etag和if-None-Match。Expires不稳定，再加入一个max-age来加以替代

### 缓存改进方案
+ md5/hash缓存
    + 通过不缓存html，为静态文件添加MD5或者hash标识，解决浏览器无法跳过缓存过期时间主动感知文件变化的问题
+ CDN缓存
    + CDN是构建在网络上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡，内容分发，调度等功能模块，使用户就近获取所需内容，降低网络阻塞，提高用户访问响应速度和命中率。

### HTTP内容协商机制
>客户端和服务器端就响应的资源内容进行交涉，然后提供给客户端最为适合的资源。内容协商会以响应资源的语言，字符集，编码方式等作为判断的基准。
+ 客户端驱动
    1. 定义：客户端发起请求，服务器发送可选项列表，客户端做出选择后在发送第二次请求
+ 服务器驱动
    1. 定义：服务器检查客户端的请求头部集并决定提供哪个版本的页面
    2. 服务器驱动内容协商-请求首部集
        + Accept：告诉服务器发送何种媒体类型
        + Accept-Language：告诉服务器发送何种语言
        + Accept-Charset：告诉服务器发送何种字符集
        + Accept-Encoding:告诉服务器采用何种编码
+ 透明协商
    1. 定义：某个中间设备（通常是缓存代理）代表客户端进行协商

### HTTP的断点续传和多线程下载
>HTTP是通过在Header里两个参数实现的，客户端发请求时对应的Range，服务器端响应时对应的是Contetn-Range

























