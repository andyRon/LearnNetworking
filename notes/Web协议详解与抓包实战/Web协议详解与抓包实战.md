# Web协议详解与抓包实战

[《Web协议详解与抓包实战》](https://time.geekbang.org/course/intro/175)笔记

课件和资源下载：https://github.com/geektime-geekbang/geektime-webprotocol



## 一、HTTP/1.1协议

### 综述

**应用层**
第 1 部分:HTTP/1.1

第 2 部分:Websocket

第 3 部分:HTTP/2.0

**应用层的安全基础设施** 

第 4 部分:TLS/SSL

**传输层**

第5部分:TCP

**网络层及数据链路层**

第 6 部分:IP 层和以太网





### 浏览器发起 HTTP 请求的典型场景

![](../../images/networking-001.jpg)



![](../../images/networking-002.jpg)



#### HTTP定义

**Hypertext Transfer Protocol (HTTP) 协议** 

a **stateless** application-level **request/response** protocol that uses **extensible semantics** and **self-descriptive** message payloads for flexible interaction with network-based **hypertext information** systems (RFC7230 2014.6)

一种**无状态的**、应用层的、以**请求/应答**方式运行的协议，它使用**可扩展的语义**和**自描述**消息格式，与基于网络的超文本信息系统灵活的互动



### 基于ABNF语义定义的HTTP消息格式

![](../../images/networking-003.jpg)

#### ABNF (扩充巴科斯-瑙尔范式)操作符

[ABNF(Augmented BNF) 官方文档](https://www.ietf.org/rfc/rfc5234.txt)

严谨的定义方式

- 空白字符:用来分隔定义中的各个元素

method SP request-target SP HTTP-version CRLF

- 选择 /:表示多个规则都是可供选择的规则 

   start-line = request-line / status-line

- 值范围 `%c##-##` :
    OCTAL=“0”/“1”/“2”/“3”/“4”/“5”/“6”/“7”与OCTAL=%x30-37等价
   
- 序列组合 ():将规则组合起来，视为单个元素

- 不定量重复 m*n:
  `*` 元素表示零个或更多元素: `*( header-field CRLF )`
  `1*` 元素表示一个或更多元素，`2*4` 元素表示两个至四个元素
  
- 可选序列 []:
 [ message-body ]

#### ABNF (扩充巴科斯-瑙尔范式)核心规则

![](../../images/networking-004.jpg)



#### 基于 ABNF 描述的 HTTP 协议格式

HTTP-message = start-line *( header-field CRLF ) CRLF [ message-body ]

- **start-line** = request-line / status-line
request-line=methodSPrequest-targetSPHTTP-versionCRLF
status-line=HTTP-versionSPstatus-codeSPreason-phraseCRLF

- **header-field** = field-name ":" OWS field-value OWS

  OWS=*(SP/HTAB)
  field-name = token 
  field-value = *( field-content / obs-fold )

- **message-body** = <span>*OCTET</span>





![](../../images/networking-005.jpg)

```
GET /wp-content/plugins/Pure-Highlightjs_1.0/assets/pure-highlight.css?ver=0.1.0 HTTP/1.1
Host:www.taohui.pub
```

wireshark

### 网络为什么要分层：OSI模型与TCP/IP模型

OSI概念模型

4 传输层 负载均衡

7 层  负载均衡

![](../../images/networking-006.jpg)

#### 报文头部

![](../../images/networking-007.jpg)

wireshark 上手👨‍💻‍



### HTTP解决了什么问题？

解决 WWW 信息交互必须面对的需求: 

- 低门槛     java的applet就是高门槛被js所取代

- 可扩展性: 巨大的用户群体，超长的寿命

- 分布式系统下的 Hypermedia: 大粒度数据的网络传输 

- Internet 规模

  * 无法控制的 scalability
      不可预测的负载、非法格式的数据、恶意消息
      客户端不能保持所有服务器信息，服务器不能保持多个请求间的状态信息

  * 独立的组件部署:新老组件并存

- 向前兼容:自 1993 年起 HTTP0.9\1.0(1996)已经被广泛使用



![](../../images/networking-008.jpg)

### 评估Web架构的七大关键属性

HTTP 协议应当在以下属性中取得可接受的**均衡**:

1. **性能** Performance:影响高可用的关键因素
2. **可伸缩性** Scalability:支持部署可以互相交互的大量组件
3. **简单性** Simplicity:易理解、易实现、易验证
4. **可见性** Visiable:对两个组件间的交互进行监视或者仲裁的能力。如缓存、分层设计等
5. **可移植性** Portability:在不同的环境下运行的能力
6. **可靠性** Reliability:出现部分故障时，对整体影响的程度
7. **可修改性** Modifiability:对系统作出修改的难易程度，由可进化性、可定制性、可扩展性、可配置性、可重用性构成

#### 性能

- **网络性能** Network Performance
  Throughput 吞吐量:小于等于带宽 bandwidth 

  Overhead 开销:首次开销，每次开销

- **用户感知到的性能** User-perceived Performance
   Latency 延迟:发起请求到接收到响应的时间
   Completion 完成时间:完成一个应用动作所花费的时间

- **网络效率** Network Efficiency
   重用缓存、减少交互次数、数据传输距离更近、COD

#### 可修改性

- 可进化性 Evolvability: 一个组件独立升级而不影响其他组件
- 可扩展性 Extensibility : 向系统添加功能，而不会影响到系统的其他部分
- 可定制性 Customizability : 临时性、定制性地更改某一要素来提供服务， 不对常规客户产生影响
- 可配置性 Configurability : 应用部署后可通过修改配置提供新的功能
- 可重用性 Reusabilit : 组件可以不做修改在其他应用在使用



#### REST 架构下的 Web



### 从五种架构风格推导出HTTP的REST架构



#### 5 种架构风格

- 数据流风格   Data-flow Styles
   • 优点:简单性、可进化性、可扩展性、可配置性、可重用性

- 复制风格  Replication Styles
   • 优点:用户可察觉的性能、可伸缩性，网络效率、可靠性也可以提到提升

- 分层风格  Hierarchical Styles
   • 优点:简单性、可进化性、可伸缩性

- 移动代码风格  Mobile Code Styles
   • 优点:可移植性、可扩展性、网络效率

- 点对点风格  Peer-to-Peer Styles
   • 优点:可进化性、可重用性、可扩展性、可配置性



#### 数据流风格 Data-flow Styles

- 管道与过滤器 Pipe And Filter，PF
   • 每个 Filter 都有输入端和输出端，只能从输入端读取数据，处理后再从输出端产生数据

  ![](../../images/networking-009.jpg)

- 统一接口的管道与过滤器 Uniform Pipe And Filter，UPF
   • 在 PF 上增加了统一接口的约束，所有 Filter 过滤器必须具备同样的接口

#### 复制风格 Replication Styles

- 复制仓库 Replicated Repository,  **RR**
   • 多个进程提供相同的服务，通过反向代理对外提供集中服务

- **缓存 $**
   • RR的变体，通过复制请求的结果，为后续请求复用

![](../../images/networking-010.jpg)

#### 分层风格Hierarchical Styles (一)

- 客户端服务器 Client-Server，**CS**
 • 由 Client 触发请求，Server 监听到请求后产生响应，Client 一直等待收到响应后，会话结束 

 • 分离关注点隐藏细节，良好的简单性、可伸缩性、可进化性

- 分层系统 Layered System ，**LS**
   • 每一层为其之上的层服务，并使用在其之下的层所提供的服务，例如 TCP/IP

- 分层客户端服务器 Layered Client-Server ， **LCS**
   • LS+CS，例如正向代理和反向代理，从空间上分为外部层与内部层
   
- 无状态、客户端服务器 Client-Stateless-Server  **CSS**

   • 基于 CS，服务器上不允许有session state会话状态
   • 提升了可见性、可伸缩性、可靠性，但重复数据导致降低网络性能

- 缓存、无状态、客户端服务器 Client-Cache-Stateless-Server **C$SS** 

   • 提升性能

- 分层、缓存、无状态、客户端服务器 Layered-Client-Cache-Stateless-Server, **LC$SS**

##### LC$SS 架构

![](../../images/networking-011.jpg)

#### 分层风格 Hierarchical Styles (二)

- 远程会话 Remote Session, **RS**
   • CS 变体，服务器保存 Application state 应用状态 

  • 可伸缩性、可见性差

- 远程数据访问 Remote Data Access ， **RDA**
   • CS 变体， Application state 应用状态同时分布在客户端与服务器 

   • 巨大的数据集有可能通过迭代而减少
   • 简单性、可伸缩性差

#### 移动代码风格 Mobile Code Styles

- 虚拟机 Virtual Machine， **VM** 

  • 分离指令与实现

- 远程求值 Remote Evaluation， **REV**

  • 基于 CS 的 VM，将代码发送至服务器执行

- 按需代码 Code on Demand， **COD**
   • 服务器在响应中发回处理代码，在客户端执行
   • 优秀的可扩展性和可配置性，提升用户可察觉性能和网络效率

- 分层、按需代码、缓存、无状态、客户端服务器 

  Layered-Code-on-Demand-Client-Cache-Stateless-Server， **LCODC$SS**

  • LC$SS+COD

- 移动代理 Mobile Agent， **MA**

  • 相当于 REV+COD

##### 统一接口的 LC$SS+COD

![](../../images/networking-012.jpg)



代码与执行过程或者执行结果分离，例如 Javascript

#### 点对点风格 Peer-to-Peer Styles

- Event-based Integration ，**EBI**:

  基于事件集成系统，如由类似 Kafka 这样的消息系统 + 分发订阅来消除耦合

  优秀的可重用性、可扩展性、可进化性

  缺乏可理解性

  由于消息广播等因素造成的消息风暴，可伸缩性差

  • **Chiron-2,C2** 

  参见论文《A Component- and Message-Based Architectural Style for GUI Software》

    相当于 EBI+LCS，控制了消息的方向 

- Distributed Objects ，DO

  组件结对交互

- Brokered Distributed Objects ，BDO
   引入名字解析组件来简化 DO，例如 CORBA



#### 风格演化

![](../../images/networking-013.jpg)

###  如何用Chrome的Network面板分析HTTP报文

https://developers.google.com/web/tools/chrome-devtools/network/

#### Network 面板

![](../../images/networking-014.jpg)



- **控制器**: 控制面板的外观与功能
- **过滤器**: 过滤请求列表中显示的资源
  按住 Command (Mac)或 Ctrl (Window / Linux)，然后点击过滤器可以 同时选择多个过滤器
- **概览**: 显示 HTTP 请求、响应的时间轴
- **请求列表**: 默认时间排序，可选择显示列
- **概要**: 请求总数、总数据量、总花费时间、DOM加载时间等



#### 控制器

- 抓包
- 停止抓包
- 清除请求
- 要跨页面加载保存请求:  **Preserve log**。  就是调整另个一页面后，前一个页面的请求还在
- 屏幕截图:Capture screenshots
- 重新执行XHR 请求:右键点击请求选择 Replay XHR
- 停用浏览器缓存
- 手动清除浏览器缓存:右键点击请求选择 Clear Browser Cache
- 离线模拟:Offline
- 模拟慢速网络连接:Network Throttling，**可自定义网速**
- 手动清除浏览器 Cookie:右键点击请求选择Clear Browser Cookies
- 隐藏 Filters 窗格
- 隐藏 Over view 窗格

#### 过滤器:按类型

- XHR、JS、CSS、Img、Media、Font、Doc、WS (WebSocket)、Manifest 或 Other (此处未列出的任何其他类型)
- 多类型，按住 Command (Mac) 或 Ctrl(Windows、Linux)
- 按时间过滤:  概览面板，拖动滚动条
- 隐藏Data URLs:  CSS 图片等小文件以 **BASE64** 格式嵌入 HTML 中，以减少 HTTP 请求数

#### 过滤器:属性过滤(一)

- domain:  仅显示来自指定域的资源。 您可以使用通配符字符 (*) 纳入多个域
- has-response-header: 显示包含指定 HTTP 响应标头的资源
- is:  使用 is:running 可以查找 WebSocket 资源，is:from-cache 可查找缓存读出的资源
- larger-than:  显示大于指定大小的资源(以字节为单位)。 将值设为 1000 等同于设置为1k
- method:  显示通过指定 HTTP 方法类型检索的资源
- mime-type:  显示指定 MIME 类型的资源

**多属性间通过空格实现 AND 操作**

#### 过滤器:属性过滤(二)

- mixed-content:显示所有混合内容资源 (mixed-content:all)，或者仅显示当前显示的资源 (mixed-content:displayed)。
- scheme:显示通过未保护 HTTP (scheme:http) 或受保护 HTTPS (scheme:https) 检索的资 源。
- set-cookie-domain:显示具有 Set-Cookie 标头并且 Domain 属性与指定值匹配的资源。
- set-cookie-name:显示具有 Set-Cookie 标头并且名称与指定值匹配的资源。
- set-cookie-value:显示具有 Set-Cookie 标头并且值与指定值匹配的资源。
- status-code:仅显示 HTTP 状态代码与指定代码匹配的资源。



#### 请求列表的排序

- 时间排序，默认

- 按列排序

- 按活动时间排序
   • Start Time:发出的第一个请求位于顶部
   • Response Time:开始下载的第一个请求位于顶部
   • End Time:完成的第一个请求位于顶部
   • Total Duration:连接设置时间和请求/响应时间最短的请求位于顶部 

   • Latency:等待最短响应时间的请求位于顶部

右击请求列表头部，可以控制显示那些列，waterfall可以选择上面不动活动时间



#### 请求列表(一)

- Name : 资源的名称

- Status : HTTP 状态代码

- Type : 请求的资源的 MIME 类型

- **Initiator** : 发起请求的对象或进程。它可能有以下几种值:
   • **Parser (解析器)** : Chrome的 HTML解析器发起了请求

   	鼠标悬停显示 JS 脚本
   • **Redirect (重定向)** : HTTP 重定向启动了请求 

   • **Script (脚本)** : 脚本启动了请求

   • **Other (其他)** : 一些其他进程或动作发起请求，例如用户点击链接跳转到页面或在地址栏中输入网址

- Size : 服务器返回的响应大小(包括头部和包体)，可显示解压后大小 
- Time : 总持续时间，从请求的开始到接收响应中的最后一个字节
- Waterfall:各请求相关活动的直观分析图



#### 预览请求内容（一）

• 查看头部
 • 查看 cookie
 • 预览响应正文:查看图像用
 • 查看响应正文
 • 时间详细分布
 • 导出数据为 HAR 格式
 • 查看未压缩的资源大小:Use Large Request Rows

#### 预览请求内容(二)

- 浏览器加载时间(概览、概要、请求列表)
   • DOMContentLoaded 事件的颜色设置为**蓝色**，而 load 事件设置为**红色**

- 将请求数据复制到剪贴版  （右击）
   • Copy Link Address: 将请求的网址复制到剪贴板
   • Copy Response: 将响应包体复制到剪贴板
   • Copy as cURL: 以 cURL 命令形式复制请求
   • Copy All as cURL: 以一系列 cURL 命令形式复制所有请求 

   • Copy All as HAR: 以 HAR 数据形式复制所有请求

- 查看请求上下游:按住 shift 键悬停请求上，绿色是上游，红色是下游



#### 浏览器加载时间

- 触发流程:
   • 解析 HTML 结构
   • 加载外部脚本和样式表文件
   • 解析并执行脚本代码 // 部分脚本会阻塞页面的加载 

   • DOM 树构建完成    // DOMContentLoaded 事件
   • 加载图片等外部文件
   • 页面加载完毕   // load 事件



#### 请求时间详细分布

- **Queueing**: 浏览器在以下情况下对请求排队
   • 存在更高优先级的请求
   • 此源已打开六个 TCP 连接，达到限值，仅适用于 HTTP/1.0 和 HTTP/1.1 • 浏览器正在短暂分配磁盘缓存中的空间

- **Stalled**: 请求可能会因 Queueing 中描述的任何原因而停止 
- **DNS Lookup**: 浏览器正在解析请求的 IP 地址
- **Proxy Negotiation**: 浏览器正在与代理服务器协商请求

- **Request sent**: 正在发送请求
- **ServiceWorker Preparation**: 浏览器正在启动 Service Worker
- **Request to ServiceWorker**: 正在将请求发送到 Service Worker
- **Waiting (TTFB)**: 浏览器正在等待响应的第一个字节。 TTFB 表示 Time To First Byte (至第一字节的时间)。 此时间包括 1 次往返延迟时间及服务器准备响应所用的时 间
- **Content Download**: 浏览器正在接收响应
- **Receiving Push**: 浏览器正在通过 HTTP/2 服务器推送接收此响应的数据
- **Reading Push**: 浏览器正在读取之前收到的本地数据

![](../../images/networking-015.jpg)



### URI的基本格式以及与URL的区别

#### 当没有 URI 时

- 站长 A 欲分享一部电影 Forrest Gump 给 B，需要告诉: • 请使用 FTP 协议访问 mysite.net，端口是 8502
   • 登录用户名是user，密码pass
   • 进入到 /shared/movie 目录下

   • 转换为二进制模式
   • 下载名为 Forrest Gump.mkv 格式的文件

- 有了 URI:ftp://user:pass@mysite.net:8502/shared/movie/Forrest Gump.mkv

#### 什么是 URI

- **URL**:  RFC1738 (1994.12)，Uniform Resource Locator，表示资源的位置， 期望提供查找资源的方法

- **URN**:  RFC2141 (1997.5)，Uniform Resource Name，期望为资源提供持久 的、位置无关的标识方式，并允许简单地将多个命名空间映射到单个URN命名 空间

  • 例如磁力链接 magnet:?xt=urn:sha1:YNCKHTQC5C

- **URI**:  RFC1630 (1994.6)、RFC3986 (2005.1，取代 RFC2396 和 RFC2732 )，Uniform Resource Identifier，用以区分资源，是 URL 和 URN 的超集，<u>用以取代 URL 和 URN 概念</u>

#### Uniform Resource Identifier 统一资源标识符

- **Resource 资源**

  可以是图片、文档、今天杭州的温度等，也可以是不能通过互联网访问的**实体**，例如人、公司、实体书，也可以是**抽象**的概念，例如亲属关系或者数字符号

  一个资源可以有多个 URI

- **Identifier 标识符**
   • 将当前资源与其他资源区分开的名称

- **Uniform 统一**
   • 允许不同种类的资源在同一上下文中出现
   • 对不同种类的资源标识符可以使用同一种语义进行解读
   • 引入新标识符时，不会对已有标识符产生影响
   • 允许同一资源标识符在不同的、internet 规模下的上下文中出现

#### URI 的组成

• 组成:schema, user information, host, port, path, query, fragment

![](../../images/networking-016.jpg)



https://tools.ietf.org/html/rfc7231?test=1#page-7

#### 合法的 URI

• ftp://ftp.is.co.za/rfc/rfc1808.txt
 • http://www.ietf.org/rfc/rfc2396.txt
 • ldap://[2001:db8::7]/c=GB?objectClass?one
 • mailto:John.Doe@example.com
 • news:comp.infosystems.www.servers.unix tel:+1-816-555-1212 • telnet://192.0.2.16:80/

• urn:oasis:names:specification:docbook:dtd:xml:4.1.2



#### URI 格式(一)

• URI = scheme ":" hier-part [ "?" query ] [ "#" fragment ] 

• scheme=ALPHA*(ALPHA/DIGIT/"+"/"-"/".")

• 例如:http, https, ftp,mailto,rtsp,file,telnet • query=*(pchar/"/"/"?")
 • fragment=*(pchar/"/"/"?")

示例: https://tools.ietf.org/html/rfc7231?test=1#page-7

#### URI 格式(二)

hier-part = "//" authority path-abempty / path-absolute / path-rootless / path-empty 

​	• authority = [ userinfo "@" ] host [ ":" port ]

​		 • userinfo = *( unreserved / pct-encoded / sub-delims / ":" ) • host = IP-literal / IPv4address / reg-name
​		 • port = *DIGIT

示例: https://tom:pass@localhost:8080/index.html

#### URI格式(三): hier-part

• path = path-abempty/ path-absolute/ path-noscheme / path-rootless / path-empty 

​	• path-abempty = *( “/” segment )

​		• 以/开头的路径或者空路径
​	• path-absolute = “/” [ segment-nz *( “/” segment ) ]

​		• 以/开头的路径，但不能以//开头
 	• path-noscheme = segment-nz-nc *( “/” segment )

​		• 以非:号开头的路径
 	• path-rootless = segment-nz *( “/” segment )

​		• 相对path-noscheme，增加允许以:号开头的路径 

​	• path-empty = 0<pchar>

​		• 空路径



#### 相对 URI

URI-reference = URI / relative-ref
 • relative-ref = relative-part [ "?" query ] [ "#" fragment ]

​	• relative-part = "//" authority path-abempty / path-absolute / path-noscheme / path-empty



https://tools.ietf.org/html/rfc7231?test=1#page-7

/html/rfc7231?test=1#page-7



### 为什么要对URI进行编码？

- 传递数据中，如果存在用作分隔符的保留字符怎么办?

- 对可能产生歧义性的数据编码 

  • 不在 ASCII 码范围内的字符 
  • ASCII 码中不可显示的字符
  • URI 中规定的保留字符
  • 不安全字符(传输环节中可能会被不正确处理)，如空格、引号、尖括号等

**示例:**

`https://www.baidu.com/s?wd=?#! `

`https://www.baidu.com/s?wd=极客 时间`

`https://www.baidu.com/s?wd=极客 ‘>时 间`

#### 保留字符与非保留字符

##### 保留字符

reserved = gen-delims / sub-delims
 	• gen-delims=":"/"/"/"?"/"#"/"["/"]"/"@"
 	• sub-delims="!"/"$"/"&"/"'"/"("/")"/"*"/"+"/","/";"/"="

##### 非保留字符

unreserved = ALPHA / DIGIT / "-" / "." / "_" / "~" 

​	 • ALPHA: %41-%5A and %61-%7A
​	 • DIGIT: %30-%39
​	 • -:%2D .:%2E _:%5F
​	 • ~: %7E，某些实现将其认为保留字符



#### URI 百分号编码

- 百分号编码的方式
   • pct-encoded = "%" HEXDIG HEXDIG

  ​	• US-ASCII:128 个字符(95 个可显示字符，33 个不可显示字符)

  ​	• 参见:https://zh.wikipedia.org/wiki/ASCII 

   • 对于 HEXDIG 十六进制中的字母，大小写等价

- 非 ASCII 码字符(例如中文):建议先 UTF8 编码，再 US-ASCII 编码 • 对 URI 合法字符，编码与不编码是等价的

- 例如，“URI 转换”既可以“URI%e8%bd%ac%e6%8d%a”，也可以 “%55%52%49%e8%bd%ac%e6%8d%a2”

  • https://www.baidu.com/s?wd=URI%20%e8%bd%ac%e6%8d%a2
  • https://www.baidu.com/s?wd=%55%52%49%20%e8%bd%ac%e6%8d%a2



### 详解HTTP的请求行

#### 请求行

request-line = method SP request-target SP HTTP-version CRLF

![](../../images/networking-017.jpg)

method 方法:指明操作目的，动词
**request-target = origin-form / absolute-form / authority-form / asterisk-form**

- origin-form = absolute-path [ "?" query ]
	向 origin server 发起的请求，path 为空时必须传递 /

- absolute-form = absolute-URI
   • 仅用于向正向代理 proxy 发起请求时，详见正向代理与隧道

- authority-form = authority
   • 仅用于 CONNECT 方法（如：**VPN**等隧道），例如 CONNECT www.example.com:80 HTTP/1.1

- asterisk-form = "*“
 • 仅用于 OPTIONS 方法

HTTP-version 版本号发展历史:https://www.w3.org/Protocols/History.html 

- HTTP/0.9:只支持 GET 方法，过时
- HTTP/ 1.0:RFC1945，1996， 常见使用于代理服务器(例如 Nginx 默认配置)
- HTTP/ 1.1:RFC2616，1999

- HTTP/ 2.0:2015.5 正式发布

#### 常见方法(RFC7231)

- GET:主要的获取信息方法，大量的**性能优化**都针对该方法，**幂等方法**（对分布式系统中**事务设计**很有意义）
- HEAD:类似 GET 方法，但服务器不发送 BODY，用以获取 HEAD 元数据，幂等方法 
- POST:常用于提交 HTML FORM 表单、新增资源等
- PUT:更新资源，带条件时是幂等方法
- DELETE:删除资源，幂等方法 

- CONNECT:建立 tunnel 隧道

- OPTIONS:显示服务器对访问资源支持的方法，幂等方法

  ```shell
  $ curl static.taohui.tech -X OPTIONS -I
  HTTP/1.1 200 OK
  Server: openresty/1.15.8.1
  Date: Sun, 22 Mar 2020 02:43:44 GMT
  Content-Length: 0
  Connection: keep-alive
  DAV: 2
  Allow: GET,HEAD,PUT,DELETE,MKCOL,COPY,MOVE,PROPFIND,OPTIONS,LOCK,UNLOCK
  ```

  

- TRACE:回显服务器收到的请求，用于定位问题。有安全风险。2007nginx不再支持

  ```
  Changes with nginx 0.5.17
   *) Change: now nginx always returns the 405 status for the TRACE method.
  ```

  

#### 用于文档管理的 WEBDAV 方法(RFC2518)

Restful  

- PROPFIND:从 Web 资源中检索以 XML 格式存储的属性。它也被重载，以允许一个检索远程系统的集合结构(也叫目录层次结构)
- PROPPATCH:在单个原子性动作中更改和删除资源的多个属性
- MKCOL:创建集合或者目录
- COPY:将资源从一个 URI 复制到另一个 URI
- MOVE:将资源从一个 URI 移动到另一个 URI
- LOCK:锁定一个资源。WebDAV 支持共享锁和互斥锁。
- UNLOCK:解除资源的锁定

#### WEBDAV 验证 !!

https://time.geekbang.org/course/detail/100026801-94392  

07:10 处



右击 > Follow > HTTP Stream

![](../../images/networking-018.jpg)

### HTTP的正确响应码

#### HTTP 响应行

status-line = HTTP-version SP status-code SP reason-phrase CRLF

![](../../images/networking-019.jpg)

#### 响应码分类:1xx

响应码规范:RFC6585 (2012.4)、RFC7231 (2014.6)。（一些服务器或中间件会自定义新的响应码）
1xx：请求已接收到，需要进一步处理才能完成，HTTP1.0 不支持

- **100 Continue**:上传大文件前使用
   • 由客户端发起请求中携带 Expect: 100-continue 头部触发

- **101 Switch Protocols**:协议升级使用
   • 由客户端发起请求中携带 Upgrade: 头部触发，如升级 websocket 或者 http/2.0

- **102 Processing**:WebDAV 请求可能包含许多涉及文件操作的子请求，需要很长时间 才能完成请求。该代码表示服务器已经收到并正在处理请求，但无响应可用。这样可 以防止客户端超时，并假设请求丢失

#### 响应码分类: 2xx

**2xx:成功处理请求**

- **200 OK**: 成功返回响应。
- **201 Created**: 有新资源在服务器端被成功创建。

- **202 Accepted**: 服务器接收并开始处理请求，但请求未处理完成。这样一个**模糊的概念**是有意如此设计，可以覆盖更多的场景。例如**异步、需要长时间处理**的任务。

- **203 Non-Authoritative Information**: 当代理服务器修改了 origin server 的原始响应包体时(例如更换了HTML中的元素值)，代理服务器可以通过修改 200为203的方式告知客户端这一事实，方便客户端为这一行为作出相应的处理。 203响应可以被缓存。

- **204 No Content**:成功执行了请求且不携带响应包体，并暗示客户端无需 更新当前的页面视图。

- **205 Reset Content**:成功执行了请求且不携带响应包体，同时指明客户端 需要更新当前页面视图。

- **206 Partial Content**:使用 **range协议**时返回部分响应内容时的响应码

- **207 Multi-Status**:RFC4918 ，在 WEBDAV 协议中以 XML 返回多个资源

  的状态。

- **208 Already Reported:**RFC5842 ，为避免相同集合下资源在207响应码 下重复上报，使用 208 可以使用父集合的响应码。

#### 响应码分类: 3xx

3xx:重定向使用 Location 指向的资源或者缓存中的资源。在 RFC2068 中规定客户端重定向次数不应超过 5 次，以防止死循环。

- 300 Multiple Choices:资源有多种表述，通过 300 返回给客户端后由其 自行选择访问哪一种表述。由于缺乏明确的细节，300 很少使用。

- 301 Moved Permanently:资源永久性的重定向到另一个 URI 中。 
- 302 Found:资源临时的重定向到另一个 URI 中。
- 303 See Other:重定向到其他资源，常用于 POST/PUT 等方法的响应中。
- 304 Not Modified:当客户端拥有可能过期的缓存时，会携带缓存的标识 etag、时间等信息询问服务器缓存是否仍可复用，而304是告诉客户端可以 复用缓存。
- 307 Temporary Redirect:类似302，但明确重定向后请求方法必须与原 请求方法相同，不得改变。
- 308 Permanent Redirect:类似301，但明确重定向后请求方法必须与原请 求方法相同，不得改变。



### HTTP的错误响应码

#### 响应码分类: 4xx

4xx:客户端出现错误

- 400 Bad Request:服务器认为客户端出现了错误，但不能明确判断为以下哪种错误

  时使用此错误码。例如HTTP请求格式错误。

- 401 Unauthorized:用户认证信息缺失或者不正确，导致服务器无法处理请求。

- 407 Proxy Authentication Required:对需要经由代理的请求，认证信息未通过代理 服务器的验证

- 403 Forbidden:服务器理解请求的含义，但没有权限执行此请求

- **404 Not Found**:服务器没有找到对应的资源

- 410 Gone:服务器没有找到对应的资源，且明确的知道该位置永久性找不到该资源

- 405 Method Not Allowed:服务器不支持请求行中的 method 方法

  ```shell
  $ curl www.sina.com.cn -X TRACE -I
  HTTP/1.1 405 Not Allowed
  Server: nginx
  Date: Sun, 22 Mar 2020 03:20:12 GMT
  Content-Type: text/html
  Content-Length: 150
  Connection: close
  X-Via-CDN: f=edge,s=cmcc.nanjing.union.237.nb.sinaedge.com,c=223.67.12.45;
  ```

  

- 406 Not Acceptable:对客户端指定的资源表述不存在(例如对语言或者编码有要

  求)，服务器返回表述列表供客户端选择。

- 408 Request Timeout:服务器接收请求超时

- 409 Conflict:资源冲突，例如上传文件时目标位置已经存在版本更新的资源

- 411 Length Required:如果请求含有包体且未携带 Content-Length 头部，且不属于 chunk类请求时，返回 411

- 412 Precondition Failed:复用缓存时传递的 If-Unmodified-Since 或 If- None-Match 头部不被满足

- 413 Payload Too Large/Request Entity Too Large:请求的包体超出服 务器能处理的最大长度

- 414 URI Too Long:请求的 URI 超出服务器能接受的最大长度

- 415 Unsupported Media Type:上传的文件类型不被服务器支持

- 416 Range Not Satisfiable:无法提供 Range 请求中指定的那段包体

- 417 Expectation Failed:对于 Expect 请求头部期待的情况无法满足时的 响应码

- 421 Misdirected Request:服务器认为这个请求不该发给它，因为它没有能力 处理。

- 426 Upgrade Required:服务器拒绝基于当前 HTTP 协议提供服务，通过 Upgrade 头部告知客户端必须升级协议才能继续处理。

- 428 Precondition Required:用户请求中缺失了条件类头部，例如 If-Match

- 429 Too Many Requests:客户端发送请求的速率过快

- 431 Request Header Fields Too Large:请求的 HEADER 头部大小超过限制

- 451 Unavailable For Legal Reasons:RFC7725 ，由于法律原因资源不可访问

#### 响应码分类: 5xx

5xx:服务器端出现错误

- 500 Internal Server Error:服务器内部错误，且不属于以下错误类型 
- 501 Not Implemented:服务器不支持实现请求所需要的功能
- 502 Bad Gateway:代理服务器无法获取到合法响应
- **503 Service Unavailable**:服务器资源尚未准备好处理当前请求
- 504 Gateway Timeout:代理服务器无法及时的从上游获得响应(代理服务器超时)
- 505 HTTP Version Not Supported:请求使用的 HTTP 协议版本不支持
- 507 Insufficient Storage:服务器没有足够的空间处理请求（通常不让用户知道，暴露了服务器信息）
- 508 Loop Detected:访问资源时检测到循环
- 511 Network Authentication Required:代理服务器发现客户端需要进 行身份验证才能获得网络访问权限



### 如何管理跨代理服务器的长短连接？

#### HTTP 连接的常见流程

短链接

![](../../images/networking-020.jpg)



#### 从 TCP 编程上看 HTTP 请求处理

![](../../images/networking-021.jpg)



#### 短连接与长连接

Connection 头部

 - Keep-Alive:长连接

   * 客户端请求长连接
      • Connection: Keep-Alive

   * 服务器表示支持长连接
      • Connection: Keep-Alive
   * 客户端复用连接
   * HTTP/1.1 默认支持
     + Connection: Keep-Alive 无意义 

- Close:短连接

- 对代理服务器的要求

  不转发 Connection 列出头部，该 头部仅与当前连接相关

![](../../images/networking-022.jpg)

#### Connection 仅针对当前连接有效

user agent 与 origin server 间有层层 proxy 代理

![](../../images/networking-023.jpg)



#### 代理服务器对长连接的支持

**问题:各方间错误使用了长连接**

- 客户端发起长连接
- 代理服务器陈旧，不能正确的处理请求的 Connection 头部，将客户端请求中的 Connection: Keep-Alive 原样转发给上游服务器 客户端
- 上游服务器正确的处理了 Connection 头部，在发送响应后没有关闭连接，而试图保持、复用与不认长连接的代理服务器的连接

- 代理服务器收到响应中 Connection: Keep-Alive 后不认，转发给 客户端，同时等待服务器关闭短连接
- 客户端收到了 Connection: Keep-Alive，认为可以复用长连接，继 续在该连接上发起请求
- 代理服务器出错，因为短连接上不能发起两次请求

**Proxy-Connection**

- 陈旧的代理服务器不识别该头部:退化为短连接

- 新版本的代理服务器理解该头部 
  - 与客户端建立长连接
  - 与服务器使用 Connection 替代 Proxy-Connect 头部



![](../../images/networking-024.jpg)



为了防止正向代理是很老的服务器，不识别长链接



### HTTP消息在服务器端的路由

#### Host 头部

- **Host = uri-host [ ":" port ]**
  - HTTP/1.1 规范要求，不传递 Host 头部则返回 400 错误响应码（以前域名比较少，每一个服务器IP地址仅对应一个域名，现在域名比较多）

- 为防止陈旧的代理服务器，发向正向代理的请求 **request-target** 必须以 **absolute-form** 形式出现
  - request-line = method SP request-target SP HTTP-version CRLF 
  - absolute-form = absolute-URI
    - absolute-URI = scheme ":" hier-part [ "?" query ]

#### 规范与实现间是有差距的

关于 Host 头部:https://tools.ietf.org/html/rfc7230#section-5.4

- A client MUST send a Host header field in all HTTP/1.1 request messages.
- A server MUST respond with a 400 (Bad Request) status code to any HTTP/1.1 request message that **lacks** a Host header field and to any request message that contains **more than one** Host header field or a Host header field with an **invalid field-value**.

#### Host 头部与消息的路由

1. 建立 TCP 连接

   确定服务器的 IP 地址

2. 接收请求

3. 寻找虚拟主机

   匹配 Host 头部与域名

4. 寻找 URI 的处理代码 

   匹配URI 

•

• 匹配URI 

5. 执行处理请求的代码

   访问资源

6. 生成 HTTP 响应

   各中间件基于 PF 架构串行修改响应

7. 发送 HTTP 响应 
8. 记录访问日志



### 代理服务器转发消息时的相关头部

#### 客户端与源服务器间存在多个代理

![](../../images/networking-025.jpg)



#### 消息的转发

- Max-Forwards 头部
  - 限制 Proxy 代理服务器的最大转发次数，仅对 TRACE/OPTIONS 方法有效 
  - Max-Forwards = 1*DIGIT

* Via 头部

  * 指明经过的代理服务器名称及版本

  * Via = 1#( received-protocol RWS received-by [ RWS comment ] )

    received-protocol = [ protocol-name "/" ] protocol-version

    received-by = ( uri-host [ ":" port ] ) / pseudonym

    pseudonym = token

- Cache-Control:no-transform
 禁止代理服务器修改响应包体



#### 如何传递 IP 地址?

![](images/image-20200322161957451.png)

### 请求与响应的上下文

#### 请求的上下文: User-Agent

指明客户端的类型信息，服务器可以据此对资源的表述做抉择

- `User-Agent = product *( RWS ( product / comment ) ) *`

  `*product = token ["/" product-version]`
  `RWS=1*(SP/HTAB)`

- 例如:
  * User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:66.0) Gecko/20100101 Firefox/66.0
  * User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36

Chrome与Safari使用同样的渲染引擎



#### 请求的上下文: Referer

浏览器对**来自某一页面**的请求自动添加的头部

- Referer = absolute-URI / partial-URI

- 例如:
  Referer: https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/User-Agent

- Referer 不会被添加的场景

  来源页面采用的协议为表示本地文件的 "file" 或者 "data" URI
  当前请求页面采用的是 http 协议，而来源页面采用的是 https 协议

- 服务器端常用于统计分析、缓存优化、防盗链等功能



#### 请求的上下文: From

- 主要用于网络爬虫，告诉服务器运维人员如何通过邮件联系到爬虫的负责人

- From = mailbox ，例如: From: webmaster@example.org



#### 响应的上下文:Server

- 指明服务器上所用软件的信息，用于帮助客户端定位问题或者统计数据 

- Server = product *( RWS ( product / comment ) )

  product = token ["/" product-version]

  例如: Server: nginx ，Server: openresty/1.13.6.2

#### 响应的上下文: Allow 与 Accept-Ranges

- **Allow**:告诉客户端，服务器上该 URI 对应的资源允许哪些方法的执行

  Allow = #method。例如: Allow: GET, HEAD, PUT

- **Accept-Ranges**:告诉客户端服务器上该资源是否允许 range 请求
  Accept-Ranges = acceptable-ranges 。 例如:

• 例如:

​		Accept-Ranges: bytes   接受range请求

​		Accept-Ranges: none



### 内容协商与资源表述

#### 内容协商

每个 URI 指向的资源可以是任何事物，可以有多种不同的表述，例如一份文档可以有不同语言的翻译、不同的媒体格式、可以针对不同的浏览器提供不同的压缩编码等。

![](../../images/networking-026.jpg)

##### 内容协商的两种方式

- Proactive 主动式内容协商:
  指由客户端先在请求头部中提出需要的表述形式，而服务器根据这些请求头部提供特定的 representation 表述

  ![](../../images/networking-027.jpg)

- Reactive 响应式内容协商:  （没有统一标准，各大浏览器很少使用）

  指服务器返回 300 Multiple Choices 或者 406 Not Acceptable，由客户端 选择一种表述 URI 使用

  ![](../../images/networking-028.jpg)



#### 常见的协商要素

- 质量因子 q:内容的质量、可接受类型的优先级(不同语言的优先级)

- 媒体资源的 MIME 类型及质量因子

  Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8

  Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp, image/apng,*/*;q=0.8,application/signed-exchange;v=b3

- 字符编码:由于 UTF-8 格式广为使用， Accept-Charset 已被废弃 

  Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7

- 内容编码:主要指压缩算法

  Accept-Encoding: gzip, deflate, br

- 表述语言

  Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
  Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2

#### 国际化与本地化

- internationalization(i18n，i 和 n 间有 18 个字符)
   指设计软件时，在不同的国家、地区可以不做逻辑实现层面的修改便能够以不同的语言显示

- localization(l10n，l 和 n 间有 10 个字符)
   指内容协商时，根据请求中的语言及区域信息，选择特定的语言作为资源表述

#### 资源表述的元数据头部

- 媒体类型、编码。 content-type: text/html; charset=utf-8

- 内容编码。 content-encoding: gzip

- 语言。Content-Language: de-DE, en-CA



在Chrome中拷贝访问百度首页的curl命令，到命令中测试

![](../../images/networking-029.jpg)

```shell
$ curl 'https://www.baidu.com/' -H 'Connection: keep-alive' -H 'Upgrade-Insecure-Requests: 1' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36' -H 'Sec-Fetch-Dest: document' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9' -H 'Sec-Fetch-Site: none' -H 'Sec-Fetch-Mode: navigate' -H 'Sec-Fetch-User: ?1' -H 'Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,zh-TW;q=0.7,de;q=0.6' -H 'Cookie: BAIDUID=F9989570A3E97C2BA3ECCC641B3719BB:FG=1; BIDUPSID=F9989570A3E97C2BA3ECCC641B3719BB; PSTM=1557330526; BD_UPN=123253; BDUSS=VdmamtLVi1Odk1IM0VJazB6VmFDc1pJRzJpbH5xVkRPbmRrRkp-Z3VabUt3SVJkRVFBQUFBJCQAAAAAAAAAAAEAAABOJ4oIMTU4N18AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIozXV2KM11dL; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; MCITY=-%3A; delPer=0; BD_CK_SAM=1; PSINO=3; H_PS_PSSID=30972_1444_31125_21109_30840_30905_30823_31085; BDSVRTM=0' --compressed
```



###  HTTP包体的传输方式（1）：定长包体

#### HTTP 包体:承载的消息内容

请求或者响应都可以携带包体

- HTTP-message = start-line *( header-field CRLF ) CRLF [ message-body ]

  message-body = *OCTET:二进制字节流

以下消息不能含有包体

- HEAD 方法请求对应的响应
- 1xx、204、304 对应的响应
- CONNECT 方法对应的 2xx 响应



#### 两种传输 HTTP 包体的方式（一）

1. 发送 HTTP 消息时**已能够确定包体的全部长度** 

   使用 Content-Length 头部明确指明包体长度。Content-Length = 1*DIGIT
   用 10 进制(不是 16 进制)表示包体中的**字节**个数，且必须与实际传输的包体长度一致

   优点:**接收端处理更简单**





### HTTP包体的传输方式（2）：不定长包体

#### 两种传输 HTTP 包体的方式（二）

2. 发送 HTTP 消息时**不能确定包体的全部长度**
   使用 Transfer-Encoding 头部指明使用 **Chunk** 传输方式

   含 Transfer-Encoding 头部后 Content-Length 头部应被忽略

   优点：

   - 基于长连接持续推送动态内容
   - 压缩体积较大的包体时，不必完全压缩完(计算出头部)再发送，可以**边发送边压缩** 
   - 传递必须在包体传输完才能计算出的 Trailer 头部（之后再发送其它头部）

!!  演示示例  testsvr.py



#### 不定长包体的 chunk 传输方式

**Transfer-Encoding头部**

- transfer-coding = "chunked" / "compress" / "deflate" / "gzip" / transfer-extension

- Chunked transfer encoding 分块传输编码: Transfer-Encoding:chunked
  ```
  chunked-body = *chunk
  							last-chunk
  							trailer-part
  							CRLF
  ```

  

  -  chunk = chunk-size [ chunk-ext ] CRLF chunk-data CRLF
    - chunk-size = 1*HEXDIG: 注意这里是 16 进制而不是10进制
    - chunk-data = 1*OCTET
  - last-chunk = 1*("0") [ chunk-ext ] CRLF
  - trailer-part = *( header-field CRLF )

!! 演示

#### Trailer 头部的传输

- TE 头部:客户端在请求在声明是否接收 Trailer 头部 

  TE: trailers

- Trailer 头部:服务器告知接下来 chunk 包体后会传输哪些 Trailer 头部 

  Trailer: Date

- 以下头部不允许出现在 Trailer 的值中:
  - 用于信息分帧的首部 (例如 Transfer-Encoding 和 Content-Length)
  - 用于路由用途的首部 (例如 Host)
  - 请求修饰首部 (例如控制类和条件类的，如 Cache-Control，Max-For wards，或者 TE)
  - 身份验证首部 (例如 Authorization 或者 Set-Cookie)
  - Content-Encoding, Content-Type, Content-Range，以及 Trailer 自身

#### MIME

- **MIME( Multipurpose Internet Mail Extensions )**
- content := "Content-Type" ":" type "/" subtype *(";" parameter)
  - type := discrete-type / composite-type
    - discrete-type := "**text**" / "**image**" / "**audio**" / "**video**" / "**application**" / extension-token
    - composite-type := "message" / "multipart" / extension-token
    - extension-token := ietf-token / x-token
  - subtype := extension-token / iana-token
  - parameter := attribute "=" value
- 大小写不敏感，但通常是小写
- 例如: Content-type: text/plain; charset="us-ascii“
  详细MIME列表：https://www.iana.org/assignments/media-types/media-types.xhtml

#### Content-Disposition 头部(RFC6266)

body以附件的方式传输时

**disposition-type = "inline" | "attachment" | disp-ext-type**

- inline:指定包体是以 inline 内联的方式，作为页面的一部分展示

- attachment:指定浏览器将包体以附件的方式下载
  + 例如: Content-Disposition: attachment
  + 例如: Content-Disposition: attachment; filename=“filename.jpg”

- 在 multipart/form-data 类型应答中，Content-Disposition也可以用于子消息体部分

  如 Content-Disposition: form-data; name="fieldName"; 

  filename="filename.jpg"



###  HTML form表单提交时的协议格式

