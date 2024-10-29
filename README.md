
## 一、写在开头


前几篇博文大概介绍了什么是网络编程，以及网络编程的实战作用，今日起，我们将针对里面涉及到的重要知识点，进行详细的梳理与学习！


在整个WEB编程中，有个应用层的协议是我们无法跳过的，那就是`HTTP`，一个`超文本传输协议` 我们浏览网页的时候，它总是第一个出现，我们今天就来学习了解一下它。


## 二、HTTP


### 2\.1 HTTP的定义


HTTP是应用层的一个重要协议，中文译为超文本传输协议，是基于TCP协议之上的，主要为WEB浏览器和WEB服务器通讯所设计，可传输超文本和多媒体内容，当我们使用浏览器浏览网页的时候，我们网页就是通过 HTTP/HTTPS 请求进行加载的。


### 2\.2 HTTP 响应状态码


HTTP状态码是描述HTTP请求结果的一个特定码值，通过它我们可以快速定位到本次请求的问题出现在了哪里。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3271023/202410/3271023-20241028154252185-1008591940.png)
状态码中，基本上以阿拉伯数字1\-5作为开头，分别代表不同含义，其中1XX我们很少看到，它表示服务端正在处理接收到的请求：


1. **100 Continue**：客户端可以继续请求。通常在客户端已发送请求的初始部分后使用，表示伺服器已接收请求的初步部分，客户端应继续发送其余部分。
2. **101 Switching Protocols**：伺服器正在切换到客户端请求的协议。这在客户端请求更改协议（如从 HTTP/1\.1 切换到 HTTP/2）时使用。


**2XX（成功状态码）**


1. 200(成功) 服务器已成功处理了请求。这个状态码对servlet是缺省的，如果没有调用setStatus方法的话，就会得到 200；
2. 201 Created：请求被成功处理并且在服务端创建了一个或多个新的资源。例如，通过 POST 请求创建一个新的用户。
3. 204(无内容) 服务器成功处理了请求，未返回任何内容；
4. 205(重置内容) 服务器成功处理了请求，未返回任何内容，重置文档视图，如清除表单内容；
5. 206(部分内容) 服务器成功处理了部分 GET 请求。


**3XX（重定向状态码）**


1. 300(多种选择) 服务器根据请求可执行多种操作。服务器可根据请求者 来选择一项操作，或提供操作列表供其选择；
2. 301(永久移动) 请求的网页已被永久移动到新位置。服务器返回此响应时，会自动将请求者转到新位置；
3. 302(临时移动) 服务器目前正从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。会自动将请求者转到新位置；
4. 304(未修改) 自从上次请求后，请求的网页未被修改过，不会返回网页内容；
5. 305(使用代理) 请求者只能使用指定的代理访问请求的网页。


**4XX（客户端错误状态码）**


1. 400(错误请求) 服务器不理解请求的语法 ；
2. 401(身份验证错误) 此页要求授权；
3. 403(禁止) 服务器直接拒绝 HTTP 请求，不处理。一般用来针对非法请求；
4. 404你请求的资源未在服务端找到。比如你请求某个用户的信息，服务端并没有找到指定的用户；
5. 406(不接受) 无法使用请求的内容特性响应请求的网页；
6. 408(请求超时) 服务器等候请求时发生超时；
7. 414(请求的 URI 过长) 请求的 URI 过长，服务器无法处理。


**5XX（服务器错误状态码）**


1. 500(服务器内部错误) 服务器遇到错误，无法完成请求；
2. 503(服务不可用) 目前无法使用服务器(由于超载或进行停机维护)。通常，这只是一种暂时的状态；
3. 504(网关超时) 服务器作为网关或代理，未及时从上游服务器接收请求；
4. 505(HTTP 版本不受支持) 服务器不支持请求中所使用的 HTTP 协议版本。


### 2\.2 HTTP请求报文


根据如下报文案例，我们看一下，其中①，②和③属于请求行；④属于请求头；⑤属于报文体。
![在这里插入图片描述](https://img2024.cnblogs.com/blog/3271023/202410/3271023-20241028154252232-675027909.png)


* ① 是请求方法，HTTP/1\.1 定义的请求方法有8种：GET、POST、PUT、DELETE、PATCH、HEAD、OPTIONS、TRACE,最常的两种GET和POST；
* ② 为请求对应的URL地址，它和报文头的Host属性组成完整的请求URL；
* ③ 是协议名称及版本号；
* ④ 是HTTP的报文头，报文头包含若干个属性，格式为“属性名:属性值”，服务端据此获取客户端的信息。
* ⑤ 是报文体，它将一个页面表单中的组件值通过param1\=value1\&param2\=value2的键值对形式编码成一个格式化串，它承载多个请求参数的数据。不但报文体可以传递请求参数，请求URL也可以通过类似于“/chapter15/user.html? param1\=value1\&param2\=value2”的方式传递请求参数。


### 2\.3 HTTP VS HTTPS


我们从下图四个方面，对比分析HTTP与HTTPS。
![在这里插入图片描述](https://img2024.cnblogs.com/blog/3271023/202410/3271023-20241028154252148-41165699.png)


* **端口号：** HTTP的端口号是80，而HTTPS的端口号是443；
* **URL前缀：** HTTP 的 URL 前缀是 http://，HTTPS 的 URL 前缀是 https://；
* **安全性和资源消耗：** HTTP是运行在TCP之上的协议，传输皆为明文，客户端和服务端无法验证对方身份，而HTTPS是运行在SSL/TLS之上的，传输内容经过了对称加密，并且堆成加密的秘钥，又在服务器端进行了非对称加密，相对HTTP安全很多，不过因为这一系列的操作，也让HTTPS耗费了更多的服务器资源；
* **SEO：** 搜索引擎通常会更青睐使用 HTTPS 协议的网站，因为 HTTPS 能够提供更高的安全性和用户隐私保护。使用 HTTPS 协议的网站在搜索结果中可能会被优先显示，从而对 SEO 产生影响。


### 2\.4 HTTP不同版本


自1996年5月公布的HTTP1\.0版本开始，经历了几十年时间，HTTP已经诞生了1\.0，1\.1，2\.0,3\.0等诸多版本，顺势时代发展，不断向前进步！


#### 2\.4\.1 HTTP1\.0 VS HTTP1\.1


1. **连接方式**：HTTP1\.0 为短连接，需要使用 keep\-alive 参数建立长连接，HTTP1\.1 默认支持keep\-alive长连接。
2. **状态响应码**：HTTP1\.1在原有的基础上增加了很多的响应码，光是错误响应状态码就新增了 24 种，如100、204、409、410等。
3. **缓存机制**：在 HTTP1\.0 中主要使用 Header 里的 If\-Modified\-Since,Expires 来做为缓存判断的标准，HTTP1\.1 则引入了更多的缓存控制策略例如 Entity tag，If\-Unmodified\-Since, If\-Match, If\-None\-Match 等更多可供选择的缓存头来控制缓存策略。
4. **带宽**：HTTP1\.0无法请求部分对象内容，不能断点续传，在HTTP1\.1在请求头引入了 range 头域，它允许只请求资源的某个部分，即返回码是 206（Partial Content），这样就方便了开发者自由的选择以便于充分利用带宽和连接。
5. **Host头处理**：HTTP1\.1 引入了 Host 头字段，允许在同一 IP 地址上托管多个域名，从而支持虚拟主机的功能。而 HTTP1\.0 没有 Host 头字段，无法实现虚拟主机。


#### 2\.4\.2 HTTP1\.1 VS HTTP2\.0


1. **多路复用**：HTTP2\.0在同一个连接中允许同时传输多个请求和响应，互不干扰。而HTTP1\.1中则采用的是串行方式，每个请求和响应都需要一个连接来处理，由于浏览器为了资源损耗控制在了6\-8个TCP连接数限制，这样HTPP1\.1的处理速度大大受限。
![在这里插入图片描述](https://img2024.cnblogs.com/blog/3271023/202410/3271023-20241028154252165-1429930236.png)
2. **二进制帧**：HTTP2\.0 使用二进制帧进行数据传输，而 HTTP1\.1 则使用文本格式的报文。二进制帧更加紧凑和高效，减少了传输的数据量和带宽消耗。
3. **头部压缩**：HTTP1\.1 支持Body压缩，Header不支持压缩。HTTP2\.0 支持对Header压缩，使用了专门为Header压缩而设计的 HPACK 算法，减少了网络开销。


#### 2\.4\.3 HTTP2\.0 VS HTTP3\.0


1. **传输协议**：HTTP2\.0 是基于 TCP 协议实现的，HTTP3\.0 新增了 QUIC（Quick UDP Internet Connections） 协议来实现可靠的传输，提供与 TLS/SSL 相当的安全性，具有较低的连接和传输延迟。你可以将 QUIC 看作是 UDP 的升级版本，在其基础上新增了很多功能比如加密、重传等等。HTTP3\.0 之前名为 HTTP\-over\-QUIC，从这个名字中我们也可以发现，HTTP3\.0 最大的改造就是使用了 QUIC。
2. **建立连接**： HTTP2\.0 需要经过经典的 TCP 三次握手过程（由于安全的 HTTPS 连接建立还需要 TLS 握手，共需要大约 3 个 RTT）。由于 QUIC 协议的特性（TLS 1\.3，TLS 1\.3 除了支持 1 个 RTT 的握手，还支持 0 个 RTT 的握手）连接建立仅需 0\-RTT 或者 1\-RTT。这意味着 QUIC 在最佳情况下不需要任何的额外往返时间就可以建立新连接。
3. **头部压缩**：HTTP2\.0 使用 HPACK 算法进行头部压缩，而 HTTP3\.0 使用更高效的 QPACK 头压缩算法。
4. **容错性**：HTTP3\.0 具有更好的错误恢复机制，当出现丢包、延迟等网络问题时，可以更快地进行恢复和重传。而 HTTP2\.0 则需要依赖于 TCP 的错误恢复机制。
5. **安全性**：在 HTTP2\.0 中，TLS 用于加密和认证整个 HTTP 会话，包括所有的 HTTP 头部和数据负载。TLS 的工作是在 TCP 层之上，它加密的是在 TCP 连接中传输的应用层的数据，并不会对 TCP 头部以及 TLS 记录层头部进行加密，所以在传输的过程中 TCP 头部可能会被攻击者篡改来干扰通信。而 HTTP3\.0 的 QUIC 对整个数据包（包括报文头和报文体）进行了加密与认证处理，保障安全性。
6. **连接迁移**：HTTP3\.0 支持连接迁移，因为 QUIC 使用 64 位 ID 标识连接，只要 ID 不变就不会中断，网络环境改变时（如从 Wi\-Fi 切换到移动数据）也能保持连接。而 TCP 连接是由（源 IP，源端口，目的 IP，目的端口）组成，这个四元组中一旦有一项值发生改变，这个连接也就不能用了。


## 三、总结


好啦，今天的HTTP学习就到这里啦，其实对于java开发工程师而言，对于HTPP的了解程度，到此也就结束了，但对于网络工程师来说，HTPP是一个至关重要的知识，需要更深层次的去探究，推荐看《图解 HTTP》这本书。



 本博客参考[豆荚加速器](https://yirou.org)。转载请注明出处！
