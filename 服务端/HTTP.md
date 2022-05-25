### 1.  URL

#### 1.1 URL（统一资源定位器）

> 是专门为标识Internet网上资源位置而设的一种编址方式，我们平时说的网页地址指的即是 url

#### 1.2 URL的组成

传输协议://服务器 IP 或域名:端口/资源所在位置标识

http://www.itcast.cn/news/fskdlf/123.html

http: 超文本传输协议，提供了一种发布和接受HTML页面的方法

### 2. HTTP协议

#### 2.1 概念

> 超文本传输协议 规定了如何从网站服务器传输文本到本地浏览器，它基于客户端服务器架构工作，是客户端(用户)和服务器端(网站)请求和应答的标准
>
> 一种**无状态**的，以请求/应答的方式运行的协议，HTTP协议自身不对请求和响应之间的通信状态进行保存。也就是说在HTTP这个级别，协议对于发送过得请求或响应都不做持久化处理。
>
> 无状态也导致了新问题，HTTP/1.1虽然是无状态协议，但为了实现期望的保持状态功能，于是引进了cookies技术。
>
> HTTP 是一个基于 **TCP/IP** 通信协议来传递数据的（HTML 文件、图片文件、查询结果等）。（可靠）
>
> 它使用可扩展的语义和自描述消息格式，与基于网络的超文本信息系统灵活的互动。

**网络分层**

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/446ab073be3c2d301f5309c2ae6b8f15&showdoc=.jpg)

#### 2.2 报文

> 在http请求和响应的过程中，传递的数据叫报文，包括要传输的数据和一些附加信息，并且要遵循规定好的格式

**HTTP报文格式：**

HTTP协议的请求报文和响应报文的结构基本相同，由三大部分组成：

- 报文首部：服务器端或服务端需处理的请求或响应的内容及属性
- 空行（CR+LF）：CR（回车符），LF（换行符）
- 报文主体：实际传输的数据，他不一定是纯文本，可以是图片，视频等二进制文件

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/68aaeefb59cc0d17c93df9ba7f08331b&showdoc=.jpg)

​						请求报文（上）和响应报文（下）的结构

请求行：包含用于请求的方法，请求URI和HTTP版本

状态行：包含响应结果的状态码，原因短语和HTTP版本

首部字段：包含表示请求和响应的各种条件和属性的各类首部



**报文主体和实体主体的差异**

> 报文（message）：是HTTP通信中的基本单位，由8位组字节流组成，通过HTTP通信传输。
>
> 实体（entity）：作为请求或响应的有效载荷数据（补充项）被传输，其内容由实体首部和实体主体组成。
>
> 
>
> HTTP报文的主体用于传输请求或响应的实体主体。
>
> 通常，报文主体等于实体主体。只有当传输中进行编码操作时，实体主体的内容发生变化，才导致它和报文主体产生差异。

#### 2.3 请求行报文格式

|        |      |      |      |         |      |
| ------ | ---- | ---- | ---- | ------- | ---- |
| METHOD | 空格   | URI  | 空格   | VERSION | 换行   |

- 请求方法：如GET/HEAD/PUT/POST,表示对资源的操作
- 请求目标：通常是一个URI，标记了请求方法要操作的资源。
- 版本号：表示报文使用的HTTP协议版本。

#### 2.4 响应行报文格式

|         |      |             |      |        |      |
| ------- | ---- | ----------- | ---- | ------ | ---- |
| VERSION | 空格   | STATUS CODE | 空格   | REASON | 换行   |

- 版本号：表示报文使用的HTTP协议版本。
- 状态码：一个三位数，用代码的形式表示处理的结果，比如200是成功，500是服务器错误
- 原因：作为数字状态码的补充，是更详细的解释文字。

### 3. HTTP请求与响应处理

#### 3.1 请求参数

#### 4.2 GET请求参数

#### 4.3POST请求参数

#### 4.4静态资源

> 服务器端不需要处理，可以直接响应给客户端的资源就是静态资源，例如css，JavaScript，image文件

#### 4.5动态资源

> 相同的请求地址不同的响应资源，这种资源就是动态资源

### 4. HTTP状态码

| 状态码  | 类别                     | 原因短语          |
| ---- | ---------------------- | ------------- |
| 1XX  | Informational（信息性状态码）  | 接收的请求正在处理     |
| 2XX  | Success（成功状态码）         | 请求正常处理完毕      |
| 3XX  | Redirection（重定向状态码）    | 需要进行附加操作已完成请求 |
| 4XX  | Client Error（客户端错误状态码） | 服务器无法处理请求     |
| 5XX  | Server Error（服务器错误状态码） | 服务器请求出错       |

- **2XX 成功**
  - 200 OK	请求在服务端被正常处理了
  - 204 No Content 代表服务器接收的请求已成功处理，但在返回的响应报文中不包含实体的主体部分，无资源返回
  - 206 Partial Content  该状态表示客户端进行了范围请求，而服务器成功执行力这部分的GET请求。响应报文中包含由Content-Range 指定范围的实体内容
- **3xx 重定向**
  - 301 Moved Permanently 永久性重定向。该状态码表示请求的资源已被分配了新的URI，以后应使用资源现在所指的URI。也就是说，如果把资源对应的URI保存为书签了，这时应按Location首部字段提示的URI进行重新保存。
  - 302 Found   临时性重定向。改状态码表示请求的资源已被分配新的URI，希望用户（本次）能使用新的URi进行访问
  - 303 See Other 该状态码表示由于请求对应的资源存在另一个URI，应使用GET方法定向获取请求的资源
  - 304 Not Modified  该状态表示客户端发送附加条件的请求时，服务器端允许请求访问资源，但为符合条件请求。
  - 307 Temporary Redirect  临时重定向。与302含义相同，307遵守浏览器标准，不会从POST变成GET
- **4xx客户端错误**
  - 400 Bad Request  表示请求报文中存在语法错误。
  - 401 Unauthorized  表示发送的请求需要有通过HTTP认证的认证信息。
  - 403 Forbidden  表示请求资源被服务器拒绝了。服务端不允许访问那个资源
  - 404 Not Found   表示服务器上没有找到请求的资源
- **5xx 服务器错误**
  - 500 Internal Server Error   表明服务器端在执行请求时发生了错误。
  - 503 Service Unavailable   表明服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。

### 5.HTTP首部

#### 5.1 HTTP首部字段（头部字段）

> 在客户端与服务器之间以 HTTP 协议进行通信的过程中，无论是请求还是响应都会使用首
> 部字段，它能起到传递额外重要信息的作用。

#### 5.2  4 种 HTTP 首部字段类型

- 通用首部字段（General Header Fields）
- 请求首部字段（Request Header Fields）
- 响应首部字段（Response Header Fields）
- 实体首部字段（Entity Header Fields)



#### 5.3 HTTP/1.1 首部字段一览

**通用首部字段**

| 首部字段名             | 说明            |
| ----------------- | ------------- |
| Cache-Control     | 控制缓存的行为       |
| Connection        | 逐跳首部、连接的管理    |
| Date              | 创建报文的日期时间     |
| Pragma            | 报文指令          |
| Trailer           | 报文末尾的首部一览     |
| Transfer-Encoding | 指定报文主体的传输编码方式 |
| Upgrade           | 升级为其他协议       |
| Via               | 代理服务器的相关信息    |
| Warning           | 错误通知          |

**请求首部字段**

| 首部字段名               | 说明                                |
| ------------------- | --------------------------------- |
| Accept              | 用户代理可处理的媒体类型                      |
| Accept-Charset      | 优先的字符集                            |
| Accept-Encoding     | 优先的内容编码                           |
| Accept-Language     | 优先的语言（自然语言）                       |
| Authorization       | Web 信息认证                          |
| Expect              | 期待服务器的特定行为                        |
| From                | 用户的电子邮箱地址                         |
| Host                | 请求资源所在的服务器                        |
| If-Match            | 比较实体标记（ETag）                      |
| If-Modified-Since   | 比较资源的更新时间                         |
| If-None-Match       | 比较实体标记（与 If-Match 相反）             |
| If-Range            | 资源未更新时发送实体 Byte 的范围请求             |
| If-Unmodified-Since | 比较资源的更新时间（与 If-Modified-Since 相反） |
| Max-Forwards        | 最大传输逐跳数                           |
| Proxy-Authorization | 代理服务器要求客户端的认证信息                   |
| Range               | 实体的字节范围请求                         |
| Referer             | 对请求中 URI 的原始获取方                   |
| TE                  | 传输编码的优先级                          |
| User-Agent          | HTTP 客户端程序的信息                     |

**响应首部字段**

| 首部字段名              | 说明             |
| ------------------ | -------------- |
| Accept-Ranges      | 是否接受字节范围请求     |
| Age                | 推算资源创建经过时间     |
| ETag               | 资源的匹配信息        |
| Location           | 令客户端重定向至指定 URI |
| Proxy-Authenticate | 代理服务器对客户端的认证信息 |
| Retry-After        | 对再次发起请求的时机要求   |
| Server             | HTTP 服务器的安装信息  |
| Vary               | 代理服务器缓存的管理信息   |
| WWW-Authenticate   | 服务器对客户端的认证信息   |

**实体首部字段**

| 首部字段名            | 说明             |
| ---------------- | -------------- |
| Allow            | 资源可支持的 HTTP 方法 |
| Content-Encoding | 实体主体适用的编码方式    |
| Content-Language | 实体主体的自然语言      |
| Content-Length   | 实体主体的大小（字节）    |
| Content-Location | 替代对应资源的 URI    |
| Content-MD5      | 实体主体的报文摘要      |
| Content-Range    | 实体主体的位置范围      |
| Content-Type     | 实体主体的媒体类型      |
| Expires          | 实体主体过期的日期时间    |
| Last-Modified    | 资源的最后修改时间      |

其他还有一些非 HTTP/1.1 规范的首部字段也用的比较多，比如 Set-Cookie、Cookie 等等。

#### 5.4 HTTP/1.1 通用首部字段

#####  5.4.1 Cache-Control

>  通过指定首部字段 Cache-Control，就能操作缓存的工作机制。

```http
// Cache-Control 的指令参数是可选的，多个指令之间通过逗号分开。该首部字段在请求和响应时都可以使用。
Cache-Control: private, max-age=0, no-cache
```

- **public 指令**`  当指定使用 public 指令时，则明确表明其他用户也可利用缓存。

- **private 指令**     当指定 private 指令后响应只以特定的用户作为对象，当该对象向服务器发起请求时，服务器会返回缓存的资源。

- **no-cache 指令**      该指令的目的是为了防止使用过期的缓存资源

  ​				**如果客户端发送的请求中包含 no-cache 指令**，则表示客户端将不会接受过期的缓存过的响应。于是，中间的代理服务器必须把请求转发给源服务器。

  ​				**如果服务器返回的响应中包含 no-cache 指令**，那么缓存服务器不能对资源进行缓存，源服务器以后也将不再对缓存服务器请求中提出的资源有效性进行确认，且禁止其对资源响应进行缓存操作。

- **no-store 指令**  当使用 no-store 指令时，暗示请求或响应中包含机密信息。因此该指令规定缓存不能在本地存储请求或响应的任一部分。

- **s-maxage 指令 **     指定缓存期限和认证的指令。

- **max-age 指令** ：

  ```http
  Cache-Control: max-age=604800（单位：秒）
  ```

  **当客户端发送的请求中包含 max-age 指令时**，如果判定缓存资源的缓存时间数值比指定的数值更小，那么客户端就接收缓存的资源。另外，当指定 max-age 值为 0，那么缓存服务器通常需要将请求转发给源服务器（相当于是 no-cache）。

  **当服务器返回的响应中包含 max-age 指令时**，缓存服务器将不对资源的有效性再做确认，而 max-age 数值代表的是资源保存为缓存的最长时间



**注意**

> 从字面意义来说，很容易把 no-cache 理解为不缓存，但事实上 no-cache 代表的是不缓存过期的资源，缓存会向源服务器确认有效性后再处理资源。no-store 才是真正的不进行缓存

#####  5.4.2 Connection

这个首部字段有一下两个作用：

- 控制不再转发给代理的首部字段
- 管理持久连接  (HTTP/1.1 的所有连接默认都是长连接）

HTTP协议的初始版本中，每进行一次通信就要断开一次TCP连接，增加通信量的开销。

持久化连接的特点是，只要任意一段没有明确提出断开连接，则保持TCP连接状态。

```
Connection:Keep-Alive
```



#####  5.4.2 Pragma

> Pragma 是 HTTP/1.1 之前版本的历史遗留字段，仅作为与 HTTP/1.0的向后兼容而定义
>
> 所有的中间服务器如果都能以 HTTP/1.1 为基准，那直接采用 Cache-Control: no-cache 指定缓存的处理方式是最为理想的。

```http
// 功能一致
Pragma: no-cache
与
Cache-Control: no-cache
```

##### 5.4.3 Via

>  使用首部字段 Via 是为了追踪客户端与服务器之间的请求和响应报文的传输路径



#### 5.5 请求首部字段

##### 5.5.1 Accept

```http
Accept: text/html,application/xhtml+xml,application/xml;q=0.
```



> Accept 首部字段可通知服务器，用户代理能够处理的媒体类型及媒体类型的相对优先级。可使用 type/subtype 这种形式，一次指定多种媒体类型。

**常用媒体类型：**

- 文本文件：

  ```
  text/html,text/plain,text/css,application/xhtml+xml,application/xml...
  复制代码
  ```

- 图片文件

  ```
  image/jpg,image/gif,image/png...
  复制代码
  ```

- 视频文件

  ```
  video/mpeg,video/quicktime...
  复制代码
  ```

- 应用程序使用的二进制文件

  ```
  application/octet-stream,application/zip...
  ```

##### 5.5.2 Accept-Charset

```
Accept-Charset: iso-8859-5,unicode-1-1;q=0.8
```

该字段可用来通知服务器用户代理支持的字符集以及字符集的权重。

##### 5.5.3 Accept-Encoding    

 该字段用来告知服务器用户代理支持的内容编码及内容编码格式的优先级。

##### 5.5.4 Accept-Language

告知服务器能够处理的自然语言。

##### 5.5.5 Authorization

该字段用于告知服务器用户代理的认证信息。

##### 5.5.6 User-Agent

该字段会将创建请求的浏览器和用户代理名称的相关信息发送给服务器。当爬虫发起请求时，有可能会在该字段添加作者的地址。

#### 5.6 响应首部字段

##### 5.6.1 Accept-Ranges

告知客户端能否处理范围请求

```
Accept-Ranges:bytes // 不能处理时为none
```

##### 5.6.2 ETag

首部字段 ETag 能告知客户端实体标志。它是一种能够将资源以字符串形式做唯一性标志的方式。服务器会为每份资源分配对应的 ETag 值。

##### 5.6.3 Location

该首部字段可以将接受响应的客户端引导到某一个与其请求 URI 位置不同的资源。

基本上该字段会配合 3xx 重定向状态码一起使用。



#### 5.7 实体首部字段

##### 5.7.1 Allow

```
Allow：GET，HEAD

```

该首部字段用于通知客户端服务器能够支持的 HTTP 方法，当服务器接收到不支持的请求方法时，会返回 405 Not Allowed 状态码。

##### 5.7.2 Content-Encoding

该首部字段会告知客户端对实体的主体部分所使用的内容编码格式

##### 5.7.3 Content-Language

该首部字段用于通知客户端实体主体所使用的自然语言

##### 5.7.4 Content-Type

```
Content-Type: text/html; charset=UTF-8
```

该首部字段说明了实体主体内的媒体类型。

##### 5.7.5 Expires

该首部字段将资源失效的日期告知客户端。

当首部字段 Cache-Control 有指定 max-age 指令时，会优先处理 max-age 指令

##### 5.7.6 Last-Modified

该首部字段指明了资源的最终修改时间。

#### 5.8 为 Cookie 服务的首部字段

>  Cookie 的工作机制是用户识别及状态管理
>
>  Cookie 是浏览器访问服务器后，**服务器传给浏览器的一段数据**。

**为 cookie 服务的首部字段:**

| 首部字段名      | 说明                   | 首部类型   |
| ---------- | -------------------- | ------ |
| Set-Cookie | 开始状态管理所使用的 Cookie 信息 | 响应首部字段 |
| Cookie     | 服务器接收到的 Cookie 信息    | 请求首部字段 |

##### Set-Cookie

下面详细讲讲 Cookie 的各个属性。

- `expires`：该属性指定浏览器可以发送 Cookie 的有效期。当省略 expires 属性时，其有效期仅限于维持浏览器会话（Session）时间段内。这通常限于浏览器关闭之前。

  另外需要注意的是一旦 Cookie 从服务器端发送到客户端，服务器端就不存在可以显式删除 Cookie 的方法。只能通过覆盖已有的 Cookie 来达到删除的目的。

- `path`：该属性用于限定指定 Cookie 的发送范围的文件目录。但是有其他办法可以避开这个限制，所以不要对它抱有太大期望。

- `domain`：该属性的域名可做到与结尾匹配一致，比如指定 domain 为 example.com，此时 [www.example.com](https://link.juejin.cn?target=http%3A%2F%2Fwww.example.com%2F) 和 www2.example.com 也都可以访问 Cookie。所以不指定域名的情况下更加安全，因为默认只有当前响应的服务器的域名可以访问 Cookie。

- `secure`：该属性限制 Web 页面仅在 HTTPS 安全连接时才发送 Cookie。当省略该属性时，HTTP 和 HTTPS 协议的指定域名都可以访问 Cookie。

- `HttpOnly`：该属性使得 JavaScript 无法对 Cookie 进行读取操作。


#### 5.9 注意区别HTTP缓存，cookie，Session

https://www.jianshu.com/p/0f5f6bb425cd

### 6.从一个http请求来看网络分层原理

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/fadfec2d8342a82c90993d65d6907535&showdoc=.jpg)

1. 输入地址并确认后，浏览器对域名进行访问，浏览器对域名进行解析，如果浏览器有域名对应的DNS相关信息的缓存，有的话可以拿到服务端的IP地址，如果没有的话，会去本地的host文件查看是否进行了配置，如果host文件没有配置相关的信息，那么就会发起DNS的请求用来获取对应的服务器的IP地址。
2. 应用端会构造DNS的请求报文，应用层会调用传输层的UDP的相关协议进行数据传输，会在DNS的基础上加上UDP的请求头然后传输信息至网络层
3. 网络层会在UDP的请求报文基础上加上IP的请求头然后到数据链路层
4. 数据链路层会实现二层寻址，会加上自己的mac信息和通过网络层的ARP协议里拿到的下一步基地的mac信息一起通过物理层一起传输出去
5. 通常传到路由器，然后路由器这个三层设备最终会通过运营商的路线传输到下一个路由器地址
6. 达到服务器后信息通过相同步骤进行层层解析HTTP的请求报文，然后构造HTTP响应报文沿着相同的步骤传输至客户端




### 7.HTTPS

> 由于HTTP天生“明文”的特点，整个传输过程完全透明，任何人都能够在链路中截获，修改或者伪造请求/响应报文，数据不具有可信性。
>
> 因此就诞生了为安全而生的HTTPS协议。
>
> 使用HTTPS时，所有的HTTP请求和响应在发送到网络之前，都要进行加密。



#### 7.1 HTTP的缺点

- 通信使用明文（不加密），内容可能会被窃听

- 不验证通信方的身份，因此有可能遭遇伪装
- 无法证明报文的完整性，所以有可能已遭篡改

#### 7.2  SSL/TLS

HTTPS使用SSL（Secure Socket Layer）和TLS（Transport Layer Security）这两个协议，加密HTTP的通信内容。

HTTP+ 加密 + 认证 + 完整性保护 = HTTPS

##### 7.2.1  摘要算法

摘要算法能够把任意长度的数据“压缩”成固定长度，而且独一无二的“摘要”字符串，就好像给这段数据生成了一个数字“指纹”。任意微小的数据差异，都可以生成完全不同的摘要。所以可以把明文信息的摘要和明文一起加密进行传输，数据传输到对方之后再进行解密，重新对数据进行摘要，再对比就能发现数据有没有被篡改。这样就能保证数据的完整性了。

例如：md5("xxxx")= adsfafasvafgasd

sha1,

sha2, 

sha1 256

##### 7.2.2 对称加密

加密和解密都用同一个密钥的方式称为共享密钥加密，也被叫做对称密钥加密。如（AES，RC4，ChaCha20）

对称加密在于如何安全的发送密钥，发送密钥就有被窃听的风险，但不发送，对方就不能解密。

##### 7.2.3 非对称加密

它有两个密钥，一个叫“公钥”，一个叫“私钥”。两个密钥是不同的，公钥可以公开给任何人使用，而私钥必须严格保密，非对称加密可以解决“密钥交换”的问题。发送密文的一方使用对方公开的密钥进行加密处理，对方接收到被加密的信息后，只能使用自己的私钥进行解密。非对称密钥加密系统通常需要大量的数学运算，比较慢。如（DH，DSA，RSA，ECC）。

##### 7.2.4 HTTPS采用混合加密

SSL/TLS里使用的是混合加密方式。即把对称加密和非对称加密结合起来，两者互相取长补短，既能够高效的加密解密，又能够安全的密钥交换。大致流程如下:

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/7a8fdfa11d7fb86fbc7229bfb77eef80&showdoc=.jpg)

##### 7.2.5 SSL证书

遗憾的是，公钥加密方式还是存在一些问题。那就是无法验证公钥就是货真价实的公开密钥。比如，正准备和某服务器建立公钥加密方式下的通信时，**如何证明收到的公钥就是原本预想的那台服务器发行的公钥**。 或许在公钥传输途中，真正的公钥已经被攻击者替换掉了。

为了解决上述问题，可以使用数字认证机构（CA，Certificate Authority）和其他机构颁发的公开密钥证书。

数字认证机构业务流程：

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/df8d51cce9014bbd5f4350e410c2bcf1&showdoc=.jpg)

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/4599065ef51df136cfb9b91780e01c7b&showdoc=.jpg)

#### 7.3 为什么不一直使用 HTTPS

>  既然 HTTPS 那么安全可靠，那为何所有的 Web 网站不一直使用HTTPS ？
>
>  HTTPS 比 HTTP 要慢 2 到 100 倍
>
>  其中一个原因是，因为与纯文本通信相比，加密通信会消耗更多的
>  CPU 及内存资源。如果每次通信都加密，会消耗相当多的资源，平
>  摊到一台计算机上时，能够处理的请求数量必定也会随之减少。
>
>  因此，如果是非敏感信息则使用 HTTP 通信，只有在包含个人信息
>  等敏感数据时，才利用 HTTPS 加密通信。
>
>  除此之外，想要节约购买证书的开销也是原因之一。



### 8.基于HTTP的功能追加协议

> 在建立 HTTP 标准规范时，制订者主要想把 HTTP 当作传输 HTML 文档的协议。随着时代的发展，Web 的用途更具多样性，比如演化成在线购物网站、SNS（Social Networking Service，社交网络服务）、企业或组织内部的各种管理工具，等等。而这些网站所追求的功能可通过 Web 应用和脚本程序实现。即使这些功能已经满足需求，在性能上却未必最优，这是因为 HTTP 协议上的限制以及自身性能有限.

#### 8.1 HTTP的瓶颈

> 在 Facebook 和 Twitter 等 SNS 网站上，几乎能够实时观察到海量用户公开发布的内容，这也是一种乐趣。当几百、几千万的用户发布内容时，Web 网站为了保存这些新增内容，在很短的时间内就会发生大量的内容更新。
>
> 使用 HTTP 协议探知服务器上是否有内容更新，就必须频繁地从客户端到服务器端进行确认。如果服务器上没有内容更新，那么就会产生徒劳的通信。

若想在现有 Web 实现所需的功能，以下这些 HTTP 标准就会成为瓶颈。

- 一条连接上只可发送一个请求。
- 请求只能从客户端开始。客户端不可以接收除响应以外的指令。
- 请求 / 响应首部未经压缩就发送。首部信息越多延迟越大。
- 发送冗长的首部。每次互相发送相同的首部造成的浪费较多
- 可任意选择数据压缩格式。非强制压缩发送

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/b71e7dc857a6024470cf162f3a35fb61&showdoc=.jpg)

#### 8.2 Ajax的解决方法

Ajax（Asynchronous JavaScript and XML， 异 步 JavaScript 与 XML 技术）是一种有效利用 JavaScript 和 DOM（Document Object Model，文档对象模型）的操作，以达到局部 Web 页面替换加载的异步通信手段。和以前的同步通信相比，由于它只更新一部分页面，响应中传输的数据量会因此而减少，这一优点显而易见。

Ajax 的核心技术是名为 XMLHttpRequest 的 API，通过 JavaScript 脚本语言的调用就能和服务器进行 HTTP 通信。借由这种手段，就能从已加载完毕的 Web 页面上发起请求，只更新局部页面。

而利用 Ajax 实时地从服务器获取内容，有可能会导致大量请求产生。另外，Ajax 仍未解决 HTTP 协议本身存在的问题。

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/f8b96b77332431addaf694514fe82f84&showdoc=.jpg)

#### 8.3 Comet 的解决方法

一旦服务器端有内容更新了，Comet 不会让请求等待，而是直接给客户端返回响应。这是一种通过延迟应答，模拟实现服务器端向客户端推送（Server Push）的功能。

通常，服务器端接收到请求，在处理完毕后就会立即返回响应，但为了实现推送功能，Comet 会先将响应置于挂起状态，当服务器端有内容更新时，再返回该响应。因此，服务器端一旦有更新，就可以立即反馈给客户端。

内容上虽然可以做到实时更新，但为了保留响应，一次连接的持续时间也变长了。期间，为了维持连接会消耗更多的资源。另外，Comet也仍未解决 HTTP 协议本身存在的问题



![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/0de511640a8d06c9e8eb47ddaa27f3a9&showdoc=.jpg)

#### 8.4 使用浏览器进行全双工通信的WebSocket

一旦 Web 服务器与客户端之间建立起 WebSocket 协议的通信连接，之后所有的通信都依靠这个专用协议进行。通信过程中可互相发送JSON、XML、HTML 或图片等任意格式的数据。由于是建立在 HTTP 基础上的协议，因此连接的发起方仍是客户端，而一旦确立 WebSocket 通信连接，不论服务器还是客户端，任意一方都可直接向对方发送报文。