###http请求内容
HTTP 请求由三部分构成，分别为		

请求行		

首部

实体
##请求行
由请求方法、URL、协议版本组成
##首部
首部分为请求首部和响应首部，部分首部两种通用

通用字段

Cache-Control	控制缓存的行为

Connection	浏览器想要优先使用的连接类型，比如 keep-alive

Date	创建报文时间

Transfer-Encoding	传输编码方式

Upgrade	要求客户端升级协议

请求首部	
Accept	能正确接收的媒体类型

Accept-Charset	能正确接收的字符集

Accept-Encoding	能正确接收的编码格式列表

Accept-Language	能正确接收的语言列表

Host	服务器的域名
If-Match	两端资源标记比较

If-Modified-Since	本地资源未修改返回 304

If-None-Match	本地资源未修改返回 304（比较标记）

User-Agent	客户端信息

Range	请求某个内容的一部分

Referer	表示浏览器所访问的前一个页面

响应首部

Accept-Ranges	是否支持某些种类的范围

Age	资源在代理缓存中存在的时间

ETag	资源标识

Location	客户端重定向到某个 URL

实体首部

Allow	资源的正确请求方式

Content-Encoding	内容的编码格式

Content-Language	内容使用的语言

Content-Length	request body 长度

Content-Location	返回数据的备用地址

Content-Range	内容的位置范围

Content-Type	内容的媒体类型

Expires	内容的过期时间

Last_modified	内容的最后修改时间
##常见状态码
2XX 成功

200 OK，表示从客户端发来的请求在服务器端被正确处理

204 No content，表示请求成功，但响应报文不含实体的主体部分

3XX 重定向

301 永久性重定向，表示资源已被分配了新的 URL

302 临时性重定向，表示资源临时被分配了新的 URL

303 表示资源存在着另一个 URL，应使用 GET 方法获取资源

304 表示服务器允许访问资源，但因发生请求未满足条件的情况

307 临时重定向，和302含义类似，但是期望客户端保持请求方法不变向新的地址发出请求

4XX 客户端错误

400 bad request，请求报文存在语法错误

401 unauthorized，表示发送的请求需要有通过 HTTP 认证的认证信息

403 forbidden，表示对请求资源的访问被服务器拒绝

404 not found，表示在服务器上没有找到请求的资源

5XX 服务器错误

500 表示服务器端在执行请求时发生了错误

501 表示服务器不支持当前请求所需要的某个功能

503 表明服务器暂时处于超负载或正在停机维护，无法处理请求

Post 和 Get 的区别

Get 请求能缓存，Post 不能

Post 相对 Get 安全一点点，因为Get 请求都包含在 URL 里（当然你想写到 body 里也是可以的），且会被浏览器保存历史纪录。Post 不会，但是在抓包的情况下都是一样的。

URL有长度限制，会影响 Get 请求，但是这个长度限制是浏览器规定的，不是 RFC 规定的

Post 支持更多的编码类型且不对数据类型限制

##浏览器缓存
通常浏览器缓存策略分为两种：强缓存和协商缓存，并且缓存策略都是通过设置 HTTP Header 来实现的。

强缓存

强缓存可以通过设置两种 HTTP Header 实现：Expires 和 Cache-Control 。强缓存表示在缓存期间不需要请求，state code 为 200。

		Expires: xxxxxx
Expires 是 HTTP/1 的产物，表示资源会在 xxxxxx 后过期，需要再次请求。并且 Expires 受限于本地时间，如果修改了本地时间，可能会造成缓存失效。

		Cache-control: max-age=30
Cache-Control 出现于 HTTP/1.1，优先级高于 Expires 。该属性值表示资源会在 30 秒后过期，需要再次请求。

协商缓存

如果缓存过期了，就需要发起请求验证资源是否有更新。协商缓存可以通过设置两种 HTTP Header 实现：Last-Modified 和 ETag 

当浏览器发起请求验证资源时，如果资源没有做改变，那么服务端就会返回 304 状态码，并且更新浏览器缓存有效期。

Last-Modified 和 If-Modified-Since

Last-Modified 表示本地文件最后修改日期，If-Modified-Since 会将 Last-Modified 的值发送给服务器，询问服务器在该日期后资源是否有更新，有更新的话就会将新的资源发送回来，否则返回 304 状态码。但是 Last-Modified 存在一些弊端：

如果本地打开缓存文件，即使没有对文件进行修改，但还是会造成 Last-Modified 被修改，服务端不能命中缓存导致发送相同的资源

因为 Last-Modified 只能以秒计时，如果在不可感知的时间内修改完成文件，那么服务端会认为资源还是命中了，不会返回正确的资源

ETag 和 If-None-Match

ETag 类似于文件指纹，If-None-Match 会将当前 ETag 发送给服务器，询问该资源 ETag 是否变动，有变动的话就将新的资源发送回来。并且 ETag 优先级比 Last-Modified 高

##输入 URL 到页面渲染的整个流程
首先是 DNS 查询(DNS 的作用就是通过域名查询到具体的 IP)

TCP连接，与浏览器建立tcp三次握手

浏览器向web服务器发送http请求

服务器处理请求并返回HTTP报文

浏览器解析渲染页面:解析HTML，构建DOM树
解析CSS，生成CSS规则树
合并DOM树和CSS规则树，生成render树
布局render树
绘制render数、树，即绘制页面像素信息
GPU将各层合成，结果呈现在浏览器窗口中

连接结束，四次挥手


