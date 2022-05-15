# 1. 网络基础

## 1.1 IP网际协议

IP（Internet Protocol）网际协议位于网络层。

- IP协议的作用是把各种数据包传送给对方。而要保证确实传送到对方那里，则需要满足各类条件。其中两个重要的条件是IP地址和MAC地址（MediaAccess Control Address）。

- IP地址指明了节点被分配到的地址，MAC地址是指网卡所属的固定地址。IP地址可以和MAC地址进行配对。IP地址可变换，但MAC地址基本上不会更改。

## 1.2 TCP

tcp位于传输层，提供可靠的字节流服务。

- 字节流服务（Byte Stream Service）是指，为了方便传输，将大块数据分割成以报文段（segment）为单位的数据包进行管理。

- 为了准确无误地将数据送达目标处，TCP协议采用了三次握手

  - 发送端首先发送一个带SYN标志的数据包给对方。接收端收到后，回传一个带有SYN/ACK标志的数据包以示传达确认信息。最后，发送端再回传一个带ACK标志的数据包，代表“握手”结束。![img](https://gitee.com/youngstory/images/raw/master/img/00ef634c-7eb2-4688-b31b-2a01123aa593-2519832.jpg)

  - 

- DNS协议提供通过域名查找IP地址，或逆向从IP地址反查域名的服务。![img](https://gitee.com/youngstory/images/raw/master/img/9a8ac678-99b3-4b27-90a4-b0d29ab8e201-2519832.jpg)
- 绝对URI的格式  ![img](https://gitee.com/youngstory/images/raw/master/img/6d3161bb-2356-4f3f-aefe-cb27e94914a2-2519832.jpg)

# 2. HTTP

- 通过请求和响应交换达成通信

  - 请求报文是由请求方法、请求URI、协议版本、可选的请求首部字段和内容实体构成的。![img](https://gitee.com/youngstory/images/raw/master/img/7d4a0d9e-1342-4e03-b019-5c8cf9e4d726-2519832.jpg)

  - 响应报文基本上由协议版本、状态码（表示请求成功或失败的数字代码）、用以解释状态码的原因短语、可选的响应首部字段以及实体主体构成。![img](https://gitee.com/youngstory/images/raw/master/img/52908cfc-40e3-48ea-83cd-e5c6c0f3a944-2519832.jpg)

- HTTP是不保存状态的协议

  - 协议对于发送过的请求或响应都不做持久化处理。所以有了Cookie

  - Cookie 

    - Cookie会根据从服务器端发送的响应报文内的一个叫做Set-Cookie的首部字段信息，通知客户端保存Cookie。当下次客户端再往该服务器发送请求时，客户端会自动在请求报文中加入Cookie值后发送出去。服务器端发现客户端发送过来的Cookie后，会去检查究竟是从哪一个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前的状态信息。![img](https://gitee.com/youngstory/images/raw/master/img/80a7c474-3829-4f4c-9a8c-3324473df340-2519832.jpg)![img](https://api2.mubu.com/v3/document_image/f52d63c6-941c-47ec-87ee-035e7661b98d-2519832.jpg)

    

- 告知服务器意图的HTTP方法 

  - GET：获取资源 
    - GET方法用来请求访问已被URI识别的资源。指定的资源经服务器端解析后返回响应内容。

  - POST：传输实体主体 
    - post的参数通常不会保存在浏览器历史里，在提交请求时，get方法的参数直接放在URL上，而post方法则是放在body里，相比于get，post不能直接看到所提交的参数。![img](https://gitee.com/youngstory/images/raw/master/img/a7737a0a-082c-4210-a18c-d7f825c928dd-2519832.jpg)

  - PUT：传输文件 
    - PUT方法用来传输文件。就像FTP协议的文件上传一样，要求在请求报文的主体中包含文件内容，然后保存到请求URI指定的位置。

  - HEAD：获得报文首部 
    - HEAD方法和GET方法一样，只是不返回报文主体部分。用于确认URI的有效性及资源更新的日期时间等。![img](https://gitee.com/youngstory/images/raw/master/img/f4d21c6f-106e-4870-adc1-c95b379053d3-2519832.jpg)

  - DELETE：删除文件

  - OPTIONS：询问支持的方法  
    - OPTIONS方法用来查询针对请求URI指定的资源支持的方法。

  - TRACE：追踪路径 
    - 客户端通过TRACE方法可以查询发送出去的请求是怎样被加工修改/篡改的。这是因为，请求想要连接到源目标服务器可能会通过代理中转，TRACE方法就是用来确认连接过程中发生的一系列操作。

  - CONNECT：要求用隧道协议连接代理
    - 要求在与代理服务器通信时建立隧道，实现用隧道协议进行TCP通信。主要使用SSL（Secure Sockets Layer，安全套接层）和TLS（Transport Layer Security，传输层安全）协议把通信内容加密后经网络隧道传输。

- 持久连接节省通信量

  - 持久连接

    - 只要任意一端没有明确提出断开连接，则保持TCP连接状态。

    - 持久连接旨在建立1次TCP连接后进行多次请求和响应的交互

  - 管线化
    - 同时并行发送多个请求，而不需要一个接一个地等待响应了

## 2.1 报文信息

- HTTP报文内的HTTP信息

  - 编码提升传输速率

    - 分割发送的分块传输编码
      - 分块传输编码会将实体主体分成多个部分（块）。每一块都会用十六进制来标记块的大小，而实体主体的最后一块会使用“0(CR+LF)”来标记。使用分块传输编码的实体主体会由接收的客户端负责解码，恢复到编码前的实体主体。

    - 压缩传输的内容编码
      - 内容编码指明应用在实体内容上的编码格式，并保持实体信息原样压缩。内容编码后的实体由客户端接收并负责解码。

  - 发送多种数据的多部分对象集合 （首部字段里加上Content-type。）

    - MIME（Multipurpose Internet Mail Extensions，多用途因特网邮件扩展）机制，它允许邮件处理文本、图片、视频等多个不同类型的数据。 采用 多部份对象集合的方法 ，来容纳多分不同 类型的数据

      - multipart/form-data在Web表单文件上传时使用。

      - multipart/byteranges状态码206（Partial Content，部分内容）响应报文包含了多个范围的内容时使用。

  - 获取部分内容的范围请求

    如在下载的过程中，断网，可通过获取部分内容的范围请求数据 进行恢复  

    - 首部通过 range 指定 ![img](https://gitee.com/youngstory/images/raw/master/img/58115208-70cf-4cf8-b2a9-dd2fe9867492-2519832.jpg)

  - 内容协商返回最合适的内容
    - 内容协商机制是指客户端和服务器端就响应的资源内容进行交涉，然后提供给客户端最为适合的资源。内容协商会以响应资源的语言、字符集、编码方式等作为判断的基准。![img](https://gitee.com/youngstory/images/raw/master/img/49c75e80-c2ca-4c20-8462-73b7c93f6465-2519832.jpg)

## 2.2 HTTP状态码

状态码的职责是当客户端向服务器端发送请求时，描述返回的请求结果。![img](https://gitee.com/youngstory/images/raw/master/img/4fd4276c-cc22-49f7-92b2-1a21c3f8ba99-2519832.jpg)

## 2.3 HTTP首部

- 使用首部字段是为了给浏览器和服务器提供报文主体大小、所使用的语言、认证信息等内容。

- 4种HTTP首部字段类型

  - 通用首部字段

    ![img](https://gitee.com/youngstory/images/raw/master/img/f73cb250-70ed-4b9c-b235-ee37c1b2cdab-2519832.jpg)

    请求和响应都会使用的字段 

    - Cache-Control 中（操作缓存 ）

      -  public  其他用户可以利用缓存， private 表示 响应只以特定的用户作为对象

      - no-cache  不接受缓存过的响应，只要源服务器的资源    防止从缓存中返回过期的资源。

      - no-store  包含机密。不能缓存下来 

    - Connection     作用

      - 控制不再转发给代理的首部字段 

      - 管理持久连接 Keep-Alive / close 

    - Via   

      - 追踪客户端与服务器之间的请求和响应报文的传输路径。

      - 报文经过代理或网关时，会先在首部字段Via中附加该服务器的信息，然后再进行转发。

  - 请求首部字段

    ![image-20220512231845385](https://gitee.com/youngstory/images/raw/master/img/image-20220512231845385.png)

    补充了请求的附加内容、客户端信息、响应内容相关优先级等信息。
    ​

    - Authorization   告知服务器，用户代理的认证信息（证书值）

  - 响应首部字段![img](https://gitee.com/youngstory/images/raw/master/img/d83a9b3c-1066-4784-a658-6f5a168a8ca2-2519832.jpg)
    补充了响应的附加内容，也会要求客户端附加额外的内容信息。

  - 实体首部字段  ![img](https://gitee.com/youngstory/images/raw/master/img/91e1b9c8-025c-4ca7-94af-1222b3a7c0bd-2519832.jpg)
    请求和响应 中实体部分使用的字段  补充了资源内容更新时间等与实体有关的信息。   

- Cookie

  - expires属性    指定浏览器可发送Cookie的有效期。![img](https://gitee.com/youngstory/images/raw/master/img/f6951082-7527-4b55-a3f5-e2f85ea7dbb1-2519832.jpg)

  - secure属性用于限制Web页面仅在HTTPS安全连接时，才可以发送Cookie。

  - HttpOnly属性是Cookie的扩展功能，它使JavaScript脚本无法获得Cookie。其主要目的为防止跨站脚本攻击（Cross-site scripting,XSS）对Cookie的信息窃取。

# 3. Web服务器

>  即使物理层面只有一台服务器，但只要使用虚拟主机的功能，则可以假想已具有多台服务器。

- 通信数据转发程序：代理、网关、隧道

  - 代理

    - 代理服务器的基本行为就是接收客户端发送的请求后转发给其他服务器

    - 每次通过代理服务器转发请求或响应时，会追加写入Via首部信息

    - 代理分为是否使用缓存 ，和是否会修改报文

      - 缓存代理 = >  会预先将资源的副本 保存在代理服务器上， 当代理再次接收到对相同资源的请求时，就可以不从源服务器那里获取资源，而是将之前缓存的资源作为响应返回。

      - 透明代理  = >  转发请求或响应时，不对报文做任何加工的代理类型被称为透明代理（Transparent Proxy）。反之，对报文内容进行加工的代理被称为非透明代理。

  - 网关

    - 利用网关可以由HTTP请求转化为其他协议通信 

    - 利用网关能提高通信的安全性，因为可以在客户端与网关之间的通信线路上加密以确保连接的安全。

  - 隧道
    - 隧道可按要求建立起一条与其他服务器的通信线路，届时使用SSL等加密手段进行通信。隧道的目的是确保客户端能与服务器进行安全的通信。

- 缓存
  - 缓存是指代理服务器或客户端本地磁盘内保存的资源副本。利用缓存可减少对源服务器的访问，因此也就节省了通信流量和通信时间。![img](https://gitee.com/youngstory/images/raw/master/img/073aad3c-98c4-4e47-b378-86eda1601823-2519832.jpg)



# 4. HTTPS

-  HTTP+加密+认证+完整性保护![img](https://gitee.com/youngstory/images/raw/master/img/3144aa3a-580b-4884-b435-45dc4fa88ab2-2519832.jpg)

- HTTPS采用共享密钥加密和公开密钥加密两者并用的混合加密机制。![img](https://gitee.com/youngstory/images/raw/master/img/65db4607-daa3-465e-8bea-ea0c3cd9e364-2519832.jpg)

- 证明公开密钥正确性的证书

  - 服务器的运营人员向数字证书认证机构提出公开密钥的申请。数字证书认证机构在判明提出申请者的身份之后，会对已申请的公开密钥做数字签名，然后分配这个已签名的公开密钥，并将该公开密钥放入公钥证书后绑定在一起。

  - 服务器会将这份由数字证书认证机构颁发的公钥证书发送给客户端，以进行公开密钥加密方式通信。公钥证书也可叫做数字证书或直接称为证书。

  - 接到证书的客户端可使用数字证书认证机构的公开密钥，对那张证书上的数字签名进行验证，一旦验证通过，客户端便可明确两件事：一，认证服务器的公开密钥的是真实有效的数字证书认证机构。二，服务器的公开密钥是值得信赖的。![img](https://gitee.com/youngstory/images/raw/master/img/87b009cb-1c1f-477c-90bb-5412d74f012b-2519832.jpg)

# 5. 确认访问者身份的认证

- BASIC 认证

  - 当请求的资源需要BASIC认证时，服务器会随状态码401Authorization Required，返回带WWW-Authenticate首部字段的响应。该字段内包含认证的方式（BASIC）及Request-URI安全域字符串（realm）。

  - 客户端将 用户ID 和 密码 发送给 服务器 ，发送前会经过Base64处理  

  - 安全性不高，明文解码容易窃听被盗  ![img](https://gitee.com/youngstory/images/raw/master/img/08c4bbe8-aad7-42c4-8e01-ced431072e0b-2519832.jpg)

- DIGEST认证

  

  - 首部字段WWW-Authenticate内必须包含realm和nonce这两个字段的信息。客户端就是依靠向服务器回送这两个值进行认证的。

- SSL 客户端认证

  - 需要事先将客户端证书分发给客户端，且客户端必须安装此证书。

  - 步骤

    - 接收到需要认证资源的请求，服务器会发送Certificate Request报文，要求客户端提供客户端证书。

    - 用户选择 客户端证书 ，以Client Certificate 报文方式发送给服务器 

    - 服务器验证客户端证书通过后，领取证书中的公开密钥 进行HTTPS加密通信

- FormBase 认证 基于表单认证

  - 客户端会向服务器上的Web应用程序发送登录信息（Credential），按登录信息的验证结果认证。

  - Session管理及Cookie应用

    - HTTP是无状态协议，无法实现状态管理 ，所以需要使用COOKIE管理 Session 

    -  服务器会发放用以识别用户的Session ID。通过验证从客户端发送过来的登录信息进行身份认证，然后把用户的认证状态与Session ID绑定后记录在服务器端。![img](https://gitee.com/youngstory/images/raw/master/img/b7dff898-ac43-4410-bfe9-9db34f6afc95-2519832.jpg)

# 6. Web攻击技术

- 在客户端即可篡改请求![img](https://gitee.com/youngstory/images/raw/master/img/12eb00c1-91f0-43ab-95ce-1423bfdd298f-2519832.jpg)
  HTTP请求报文内加载攻击代码，就能发起对Web应用的攻击。通过URL查询字段或表单、HTTP首部、Cookie等途径把攻击代码传入，若这时Web应用存在安全漏洞，那内部信息就会遭到窃取，或被攻击者拿到管理权限。

- 对Web应用的攻击模式有以下两种

  - 主动攻击

    - 以服务器为目标的主动攻击  

      攻击者通过直接访问Web应用，把攻击代码传入的攻击模式。  

      - 具有代表性的攻击是SQL注入攻击和OS命令注入攻击。

  - 被动攻击

    - 以服务器为目标的被动攻击

      

      指利用圈套策略执行攻击代码的攻击模式。

      - 具有代表性的攻击是跨站脚本攻击和跨站点请求伪造。

    - 跨站脚本攻击（Cross-Site Scripting,XSS）

      - 通过存在安全漏洞的Web网站注册用户的浏览器内运行非法的HTML标签或JavaScript进行的一种攻击。

        - 案例  

          - 表单圈套 ： 在 地址栏中  植入  script脚本     并隐藏植入事先准备好的欺诈邮件中或Web页面内，诱使用户去点击该URL。 ![img](https://gitee.com/youngstory/images/raw/master/img/5add716e-3ee3-4fec-9ffc-83e39aff88d1-2519832.jpg)

          - 对用户Cookie的窃取攻击![img](https://gitee.com/youngstory/images/raw/master/img/c48a234d-389b-449c-9e42-d35683ce80a9-2519832.jpg)
            在存在可跨站脚本攻击安全漏洞的Web应用上执行上面这段JavaScript程序，即可访问到该Web应用所处域名下的Cookie信息。然后这些信息会发送至攻击者的Web网站（http://hackr.jp/）

      - SQL注入攻击

        - 指针对Web应用使用的数据库，通过运行非法的SQL而产生的攻击。

        - 案例

          - 通过作者姓名 查询 对应的图书 ，出版和未出版的都可以查到  

          - 正常操作 ![img](https://gitee.com/youngstory/images/raw/master/img/c7aeeb3d-b2bf-48f0-b83d-c1882611edc6-2519832.jpg)

          - 非正常操作  SQL 注入   ![img](https://gitee.com/youngstory/images/raw/master/img/02b4e361-678c-4b02-86ee-642894ca21f3-2519832.jpg)

      -  OS命令注入攻击

        指通过Web应用，执行非法的操作系统命令达到攻击的目的。只要在能调用Shell函数的地方就有存在被攻击的风险。

        - OS命令注入攻击可以向Shell发送命令，让Windows或Linux操作系统的命令行启动程序。也就是说，通过OS注入攻击可执行OS上安装着的各种程序。

      - 密码破解

        - 穷举法 （暴力破解）  

          - 用所有可行的候选密码对目标的密码系统试错，用以突破验证的一种攻击。

          - 如 4位数字密码 。就用0000-9999 逐个进行尝试 

        - 字典法

          - 指利用事先收集好的候选密码（经过各种组合方式后存入字典），枚举字典中的密码，尝试通过认证的一种攻击手法。

          - ![img](https://gitee.com/youngstory/images/raw/master/img/bfbfb808-fd76-4dc4-9464-245ec49ce062-2519832.jpg)



