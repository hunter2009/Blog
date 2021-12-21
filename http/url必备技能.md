
# URL
**URL 代表着是统一资源定位符（** *Uniform Resource Locator* **）** 。和 [Hypertext](https://developer.mozilla.org/zh-CN/docs/Glossary/Hypertext) 以及 [HTTP](https://developer.mozilla.org/zh-CN/docs/Glossary/HTTP) 一样，是 Web 中的一个核心概念。它是[浏览器](https://developer.mozilla.org/zh-CN/docs/Glossary/Browser)用来检索 web 上发布的任何资源的机制。

URL 无非就是一个给定的独特资源在 Web 上的地址。理论上说，每个有效的 URL 都指向一个唯一的资源。这个资源可以是一个 HTML 页面，一个 CSS 文档，一幅图像，等等。而在实际中，也有一些例外，最常见的情况就是一个 URL 指向了不存在的或是被移动过的资源。由于通过 URL 呈现的资源和 URL 本身由 Web 服务器处理，因此 web 服务器的拥有者需要认真地维护资源以及与它关联的URL。

下面是一个标准的URL示例：

```
http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument
```
它含有以下几部分：(参考链接：https://url.spec.whatwg.org/#valid-url-string)


![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cd71f421c2a34ef68b50744d08d84072~tplv-k3u1fbpfcp-watermark.image?)


备注：
**url 规范和 web url 比较起来存在一些差异（命名上），后面讲解时会尽量做出区分**


## scheme 方案
`http` 是方案也是协议。在web中，它也代表了协议，表明了浏览器必须使用何种通信协议协议，通常都是HTTP协议或是HTTP协议安全版（即HTTPS）。Web需要它们二者之一，但浏览器能处理其他协议，比如`mailto:（打开邮件客户端）或者 ``ftp:（处理文件传输），所以当你这些协议时，不必惊讶。`

**scheme和protocol的区别**

URL 不包含协议，但包含方案 [[1]](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)。方案可以与协议相关联，但不是必须的。

例如，在示例中，该`http:`方案与 HTTP/1.0 或 1.1 协议 [[2]](https://www.w3.org/2001/tag/doc/SchemeProtocols.html#useNaturalProtocol)`file:`相关联，但该方案不与任何协议相关联。`http`是方案和协议，而`file`是方案但不是协议。

常见的scheme&port对应关系如下

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6a582fc1ab60497d94d945de3302ea0f~tplv-k3u1fbpfcp-watermark.image?)

说明：
在web场景中，更多的会使用protocal（例如：`location.protocol`），如果看到协议，可以`等价视为scheme`

## host 主机地址
host可以是ip或者域名，在示例中的`www.example.com` 是域名。 它表明正在请求哪个Web服务器，也可以直接使用[IP address](https://developer.mozilla.org/zh-CN/docs/Glossary/IP_Address), 但因为它不太方便，所以不经常在网络上使用。

一个域名包含几部分，可能是一部分、两部分、三部分等等，每个部分被`.`分割，不同于我们正常的输入顺序，它需要`从右到左`进行阅读

常见的域名如下下：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/48fd822832ea4afe947be071cfb483d8~tplv-k3u1fbpfcp-watermark.image?)

域名的每一部分都提供着特定信息。

### [TLD (en-US)](https://developer.mozilla.org/en-US/docs/Glossary/TLD "Currently only available in English (US)") （Top-Level Domain，顶级域名）

顶级域名可以告诉用户 域名所提供的的`服务类型`。
最通用的顶级域名（.com, .org, .net）不需要web服务器满足严格的标准，但一些顶级域名确需要执行更严格的政策。比如：

-   地区的顶级域名，如`.cn，.us，.fr`或`.sh`，要求必须提供给定语言的服务器或者托管在指定国家。这些TLD通常表明对应的网页服务从属于何种语言或哪个地区。
-   包含`.gov`的顶级域名只能被政府部门使用。
-   `.edu`只能为教育或研究机构使用。

顶级域名既可以包含拉丁字母，也可以包含特殊字符。顶级域名最长可以达到63个字符，不过为了使用方便，大多数顶级域名都是两到三个字符。

顶级域名的完整列表是[ICANN](https://www.icann.org/resources/pages/tlds-2012-02-25-en)维护的。

### Label (标签或者组件)

   标签都是紧随着TLD的。标签由1到63个大小写**不敏感**的字符组成，这些字符包含字母A-z，数字0-9，甚至 “-” 这个符号（当然，“-” 不应该出现在标签开头或者标签的结尾）。举几个例子，`a`，`97`，或者 `hello-strange-person-16-how-are-you`  都是合法的标签。

### Secondary Level Domain（二级域名）

位于TLD前面的标签也被称为二级域名 (SLD)。一个域名可以有多个标签（或者说是组件），没有强制规定必须要3个标签来构成域名。例如，www.inf.ed.ac.uk 是一个正确的域名。当拥有了“上级”部分(例如 [mozilla.org](https://mozilla.org/))，你还可以创建另外的域名 (有时被称为 "子域名") (例如 [developer.mozilla.org](https://developer.mozilla.org/)).

## port 端口
`:80` 是端口。 它表示用于访问Web服务器上的资源的技术“门”。如果Web服务器使用HTTP协议的标准端口（HTTP为80，HTTPS为443）来授予其资源的访问权限，则通常会被忽略。否则是强制性的。
## path 路径
`/path/to/myfile.html` 是网络服务器上资源的路径。在Web的早期阶段，像这样的路径表示Web服务器上的物理文件位置。如今，它没有任何物理意义，仅仅是Web服务器中的一个抽象概念，需要关注的是`path`的可理解性
## query 查询
`?key1=value1&key2=value2` 是查询，也可称之为参数（paramters）。 这些参数是用 `& `符号分隔的键/值对列表。在返回资源之前，Web服务器可以使用这些参数来执行额外的操作。每个Web服务器都有自己关于参数的规则，唯一可靠的方式来知道特定Web服务器是否处理参数是通过询问Web服务器所有者。
## fragment 片段
在web中，更常见的是称之为**anchor（锚点）**

`#SomewhereInTheDocument` 是资源本身的一部分。 锚点表示资源中的一种“书签”，告知浏览器显示位于该“加书签”位置的内容。例如，在HTML文档上，浏览器将滚动到定义锚点的位置；在视频或音频文档上，浏览器将尝试转到锚代表的**时间**。

值得注意的是，`＃`后面的部分（fragment）从来**没有**发送到请求的服务器
# origin & site

## origin 源
“origin”是 [scheme](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web#Scheme_or_protocol) （也称为 [协议](https://developer.mozilla.org/en-US/docs/Glossary/Protocol)，例如[HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP)或 [HTTPS](https://developer.mozilla.org/en-US/docs/Glossary/HTTPS)协议）、 [host](https://en.wikipedia.org/wiki/Hostname)和 [port](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web#Port) （如果指定）的组合。例如，给定一个 URL `https://www.example.com:443/foo`，“来源”是`https://www.example.com:443`。

具有相同**scheme**、**host**和**port**组合的网站被视为“**同源**”（`即协议、域名、端口号一致`），其他一切都被视为“**跨源**” (也可称之为“**跨域**”)。

### 同源和跨源

| 源A | 源B | 是否同源 |
| --- | --- | -- |
| https://www.example.com:443 | https://www.evil.com:443 | 跨源：**域**不同 |
| https://www.example.com:443  | https://**example.com**:443 |  跨源：**子域**不同 |
| https://www.example.com:443  | https://**login**.example.com:443 |  跨源：**子域**不同 |
| https://www.example.com:443  | **http**://www.example.com:443 | 跨源：**方案**不同 |
| https://www.example.com:443  | https://www.example.com:80 | 跨源：**端口号**不同 |
| https://www.example.com:443  | **https://www.example.com** | 同源：**端口号**省略 |
| https://www.example.com:443  | **https://www.example.com:443** | 同源：url一致 |



## site 站点
“站点”是 TLD 与其之前的域部分的组合。在下面的实例中，给定 URL 为`https://www.example.com:443/foo`，“站点”为 `example.com`。

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2f1e60c3de1c488bb0dab3b3a5d77c82~tplv-k3u1fbpfcp-watermark.image?)

但是很多真实场景下，仅仅使用TLD是不够的，例如：`.co.jp`或者`.github.io`，它们的TLD是`.jp`或者`.io`，单在这个粒度（TLD）上基本不能标识`site`，并且无法通过算法来计算特定TLD，所以基于TLD，又提出了`effective TLD`(eTLD)，它标明了有效的TLD定义包含哪些（[查看定义（公共后缀列表）](https://wiki.mozilla.org/Public_Suffix_List)）。eTLDs列表维护在[publicsuffix.org/list](https://publicsuffix.org/list/) 中。

整个`站点名称`称为 eTLD+1。例如，如果 URL 为 `https://my-project.github.io`，则 eTLD 为`.github.io`且 eTLD+1 为 `my-project.github.io`，这被看做“站点”。换句话说，eTLD+1 是有效的 TLD 和它之前的域的一部分

### 同站和跨站
可以参考如下表格

源A                             | 源B                            | 是否同站
| ------------------------------- | ------------------------------ | ----------------------------- |
| https://www.example.com:443     | https://www.evil.com：443 | 跨站：**域**不用        
| https://www.example.com:443     | https://**login**.example.com:443  | 同站：**子域**无关 
| https://www.example.com:443     | **http**://www.example.com:443 | 同站：**方案**无关         
| https://www.example.com:443     | https://www.example.com: **80** | 同站：**端口**无关 
| https://www.example.com:443     | **https://www.example.com:443** | 同站：完全匹配     
| https://www.example.com:443     |**https://www.example.com**     | 同站：端口无关（**省略时**）

总结来说就是: ***只有域名不同*** 才属于夸张

## schemeful same-site 有计划的同站
传统的`SameSite`实际上只涵盖了域名相关的内容（**只有域名不同才属于跨站**），但是在我们日常工作中，很容易遇到https和https互相切换的情况（比如全域升级https），这个时候很容易留下安全漏洞（HTTP 被用作[弱通道](https://tools.ietf.org/html/draft-west-cookie-incrementalism-01#page-8)），例如：`setCookie`时会设置`SameSite=Lax`来防止[跨站点请求伪造 (CSRF)](https://developer.mozilla.org/docs/Glossary/CSRF)。但是，不安全的 HTTP 流量会给网络攻击者提供篡改 cookie 的机会，这些 cookie 随后将用于站点的 HTTPS 场景。


因此`schemeful same-site`应运而生，[schemeful same-site](https://web.dev/schemeful-samesite/)拥有更严格的定义。在这种情况下，`http://www.example.com`和`https://www.example.com`由于方案不匹配被视为跨站点。



源A                          | 源 B                                                   | 是否有计划的同站 |
| --------------------------------- | ---------------------------------------------------------- | --------------------------------------------------------------- |
| https://www.example.com:443       | https://www.evil.com:443                               | 跨站：**域**不同             
| https://www.example.com:443   | https://**login**.example.com:443 | 有计划的同站：**子域**无关                                                           
| https://www.example.com:443   | **http**://www.example.com:443    | 跨站: **方案**不同 
| https://www.example.com:443   | https://www.example.com:**80**    | 有计划的同站：**端口**无关                                                       
| https://www.example.com:443   | **https://www.example.com:443**   |  有计划的同站：完全匹配                                                          
| https://www.example.com:443   | **https://www.example.com**       | 有计划的同站：端口无关（**省略时**）

## 如何区分“同站” vs “跨站” 和  “同源” vs “跨源”

Chrome76+ 后（特别是90+版本后），安全机制不断发生变化，本地开发会遇到各种神奇的问题（感兴趣或者遇到问题可以私信活留言），可以通过HTTP request header `Sec-Fetch-Site`（元数据请求头）进行区分。

`Sec-Fetch-Site`截至 2020 年 4 月，尚无其他浏览器支持。这是更大的[Fetch Metadata Request Headers](https://www.w3.org/TR/fetch-metadata/) 提案的一部分。请求头具有以下值之一：

-   `cross-site`
-   `same-site`
-   `same-origin`
-   `none`

通过检查 的值`Sec-Fetch-Site`，您可以确定请求是“同一站点”、“同源”还是“跨站点”，但遗憾的是 `schemeful-same-site` 目前在`Sec-Fetch-Site`中还不支持

# fetch metadata 请求元数据
许多 Web 应用程序容易受到[跨域](https://web.dev/same-site-same-origin/#%22same-origin%22-and-%22cross-origin%22)攻击，例如[跨站点请求伪造](https://portswigger.net/web-security/csrf)(CSRF)、[跨站点脚本包含](https://portswigger.net/research/json-hijacking-for-the-modern-web)(XSSI)、定时攻击、[跨源信息泄漏](https://arxiv.org/pdf/1908.02204.pdf) 或 [Spectre](https://developers.google.com/web/updates/2018/02/meltdown-spectre) 攻击。

[Fetch Metadata](https://www.w3.org/TR/fetch-metadata/)请求头提供了更强大的防御机制  **资源隔离策略** 以保护您的应用程序免受这些常见的跨域攻击。通过在一组`Sec-Fetch-*`标头中提供有关 HTTP 请求上下文的信息，它们允许响应服务器在处理请求之前应用安全策略

**同源**

来自您自己的服务器（同源）服务的站点的请求将继续工作。
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d117cfa67bee4975950db70ae3634020~tplv-k3u1fbpfcp-watermark.image?)

**跨站**

由于`Sec-Fetch-*`标头提供的 HTTP 请求中的附加上下文，服务器可能会拒绝恶意跨站点请求。
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f3b188f863ee4ad6ad791b4e3ac539a7~tplv-k3u1fbpfcp-watermark.image?)

### [Sec-Fetch-Site](https://web.dev/fetch-metadata/#sec-fetch-site)

`Sec-Fetch-Site`告诉服务器哪个[站点](https://web.dev/same-site-same-origin)发送了请求。浏览器将此值设置为以下值之一：

-   `same-origin`，如果请求是由您自己的应用程序提出的（例如`site.example`）
-   `same-site`, 如果请求是由您网站的子域发出的（例如`bar.site.example`）
-   `none`, 如果请求是由用户与用户代理的交互（例如点击书签）明确引起的
-   `cross-site`, 如果请求是由另一个网站发送的（例如`evil.example`）

### [Sec-Fetch-Mode](https://web.dev/fetch-metadata/#sec-fetch-mode)

`Sec-Fetch-Mode`指示请求的[模式](https://developer.mozilla.org/docs/Web/API/Request/mode)。这大致对应于请求的类型，并允许您区分资源负载和导航请求。例如，目的地`navigate`表示顶级导航请求，而`no-cors`表示加载图像等资源请求。

- `cors`, 该请求是[CORS 协议](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)请求。
- `navigate`,该请求由 HTML 文档之间的导航发起。
- `no-cors`,该请求是无 cors 请求（请参阅 参考资料[`Request.mode`](https://developer.mozilla.org/en-US/docs/Web/API/Request/mode#value)）。
- `same-origin`,该请求与所请求的资源是同一来源。
- `websocket`,正在请求建立[WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)连接。

### [Sec-Fetch-Dest](https://web.dev/fetch-metadata/#sec-fetch-dest)

`Sec-Fetch-Dest`用来说明请求的[目的地](https://developer.mozilla.org/docs/Web/API/Request/destination)，也可以看出原始请求的发起者，即在何处（以及如何）使用获取的数据。

[`Sec-Fetch-Dest`](https://developer.mozilla.org/en-US/docs/Glossary/Fetch_metadata_request_header)
这允许服务器根据请求是否适合*预期的使用方式*来确定是否为请求提供服务。例如，带有`audio`目的地的请求应该请求音频数据，而不是其他类型的资源（例如，包含敏感用户信息的文档）

列举一些常见的值（不完全）

- `audio`,   目的地是音频数据。这可能源自 HTML[`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio)标记。
- `font`, 目标是字体。这可能源于 CSS [`@font-face`](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face)。
- `frame`, 目标是一个框架。这可能源自 HTML[`<frame>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/frame)标记。
- `iframe`, 目标是一个 iframe。这可能源自 HTML[`<iframe>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)标记。
- `script`,  目标是一个脚本。这可能源自 HTML[`<script>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script)标记或对[`WorkerGlobalScope.importScripts()`](https://developer.mozilla.org/en-US/docs/Web/API/WorkerGlobalScope/importScripts).
- `style`, 目的地是一种风格。这可能源自 HTML [<link rel=stylesheet>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link)或 CSS [`@import`](https://developer.mozilla.org/en-US/docs/Web/CSS/@import)。

# 总结
文章到这里就结束了，期望你看完后对`url`、`site`、`origin`、`跨域`、`跨站`等知识点有一些新的理解，如果你有任何建议也欢迎随时留言或者私信


# referrer
- https://url.spec.whatwg.org
- https://web.dev/fetch-metadata/
- https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/What_is_a_URL
- https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/What_is_a_domain_name
- https://web.dev/same-site-same-origin/
- https://web.dev/schemeful-samesite/
