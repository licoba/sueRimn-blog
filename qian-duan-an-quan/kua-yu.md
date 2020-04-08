# 跨域

跨域

#### 一、跨域（CORS）

跨域资源共享（`CORS`）是一种机制，其机制是使用一组额外响应头（`Access-Control-Allow-Origin`）和预检请求\(`OPTIONS`\)来使浏览器有权使用非同源资源。**出于安全原因，浏览器限制从脚本内发起的跨源HTTP请求**。更准确的说有两种表现：（1）限制发起`AJAX`请求\(`XMLHttpRequest`，`Fetch`\)；（2）拦截其他跨站请求的返回结果，这取决于请求是否为简单请求。说明了跨域并不能完全阻止`CSRF`，因为请求还是能发出去。

它允许浏览器向跨源服务器，发出`XMLHttpRequest`请求，从而克服了`AJAX`只能同源使用的限制。简单来说就是跨域的目标服务器要返回一系列的`Headers`，通过这些`Headers`来控制是否同意跨域。

以下标签是可以跨域加载资源的：

* `img`
* `link`
* `script`

简单来说就是避免同源策略，什么是同源呢?

**1、同源策略**

同源策略是指，浏览器内部发起请求时，该来源必须是当前同源的`HTTP`资源，如果没有同源策略，浏览器容易受到`XSS`、`CSRF`等攻击。同源指**协议+域名+端口**必须相同，其中任何一个不同，都不属于同源。

**2、需要CORS的场景**

1. 由 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 或 [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 发起的跨域 HTTP 请求
2. `Web` 字体 \(`CSS` 中通过`@font-face`使用跨域字体资源\)
3. `WebGL`贴图
4. 使用`drawImage`

`CORS`需要浏览器和服务器同时支持，整个过程是浏览器自动完成，无用户参与，浏览器一旦发现`AJAX`请求跨源，就会自动添加一些附加的头信息或者附加的请求，这个过程对用户来说是不可见的。实现`CORS`通信的关键是服务器。只要服务器实现了`CORS`接口，就可以跨源通信。

先来看看这些头部信息分别有哪些，代表什么意思。

**3、HTTP首部字段**

首部字段无须手动设置。 当开发者使用 `XMLHttpRequest`对象发起跨域请求时，它们已经被设置就绪。

**（1）HTTP请求头（首部字段）**

| HTTP请求头 | 参数 | 作用 |
| :--- | :--- | :--- |
| `Origin` | `Origin: <origin>` | 预检请求或实际请求的源站`URL`，不包含任何路径信息，只是服务器名称，不管是否为跨域请求，`Origin` 字段总是被发送 |
| `Access-Control-Request-Method` | `Access-Control-Request-Method: <method>` | 用于预检请求。其作用是，将实际请求所使用的 `HTTP`方法告诉服务器 |
| `Access-Control-Request-Headers` | `Access-Control-Request-Headers: <field-name>[, <field-name>]*` | 用于预检请求。其作用是，将实际请求所携带的首部字段告诉服务器 |

**（2）HTTP响应头（首部字段）**

| HTTP响应头 | 参数 | 作用 |
| :--- | :--- | :--- |
| `Access-Control-Allow-Origin` | `Access-Control-Allow-Origin: <origin>|*` | 该字段是必须的，`origin`参数的值指定了允许访问该资源的外域 `URI`。对于不需要携带身份凭证的请求，服务器可以指定该字段的值为通配符`*`，表示允许来自所有域的请求 |
| `Access-Control-Expose-Headers` |  | 该字段可选，让服务器把允许浏览器访问的头放入白名单。因为在跨域访问时，`XMLHttpRequest`对象的`getResponseHeader()`方法只能拿到一些最基本的响应头，`Cache-Control`、`Content-Language`、`Content-Type`、`Expires`、`Last-Modified`、`Pragma`，如果要访问其他头，则需要服务器设置本响应头 |
| `Access-Control-Max-Age` | `Access-Control-Max-Age: <delta-seconds>` | 该字段可选，指定了`preflight`请求的结果能够被缓存多久，`delta-seconds` 参数表示`preflight`请求的结果在多少秒内有效 |
| `Access-Control-Allow-Credentials` | `Access-Control-Allow-Credentials: true` | `该字段可选，值是一个布尔值，表示是否允许发送`Cookie`，指定了当浏览器的`credentials`设置为true时是否允许浏览器读取`response`的内容。当用在对`preflight`预检测请求的响应中时，它指定了实际的请求是否可以使用`credentials`。请注意：简单 GET`请求不会被预检；如果对此类请求的响应中不包含该字段，这个响应将被忽略掉，并且浏览器也不会将相应内容返回给网页。 |
| `Access-Control-Allow-Methods` | `Access-Control-Allow-Methods: <method>[, <method>]*` | 用于预检请求的响应。其指明了实际请求所允许使用的 `HTTP` 方法 |
| `Access-Control-Allow-Headers` | `Access-Control-Allow-Headers: <field-name>[, <field-name>]*` | 用于预检请求的响应。其指明了实际请求中允许携带的首部字段 |

需要注意的是，如果要发送`Cookie`，`Access-Control-Allow-Origin`就不能设为星号，必须指定明确的、与请求网页一致的域名。同时，`Cookie`依然遵循同源政策，只有用服务器域名设置的`Cookie`才会上传，其他域名的`Cookie`并不会上传，且（跨源）原网页代码中的`document.cookie`也无法读取服务器域名下的`Cookie`。

#### 二、跨域解决方案

**1、CORS**

浏览器将`CORS`请求分成两类：简单请求（`simple request`）和非简单请求（`not-so-simple request`）。浏览器对这两种请求的处理是不一样的，需要区分。

**（1）简单请求**

不会触发`CORS预检请求`的称为简单请求。简单请求必须同时满足以下两种情况：

**1）请求方法是以下三种之一：**

* `HEAD`
* `GET`
* `POST`

**2）HTTP头信息不超过以下几种字段：**

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Last-Event-ID`
* `Content-Type`：只限于三个值
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`
* 请求中的任意`XMLHttpRequestUpload` 对象均没有注册任何事件监听器
* `XMLHttpRequestUpload`对象可以使用 `XMLHttpRequest.upload`属性访问
* 请求中没有使用 `ReadableStream`对象

如果不满足以上两个条件，就不属于简单请求，属于复杂请求。

**基本流程**

对于简单请求，浏览器直接发出`CORS`请求。具体来说，浏览器发现这次跨源AJAX请求是简单请求，就在头信息之中，增加一个`Origin`字段。`Origin`字段用来说明，本次请求来自哪个源（协议 + 域名 + 端口）。服务器根据这个值，决定是否同意这次请求。

如果`Origin`指定的源，不在许可范围内，服务器会返回一个正常的HTTP回应。浏览器发现，这个回应的头信息没有包含`Access-Control-Allow-Origin`字段，就知道出错了，从而抛出一个错误，被`XMLHttpRequest`的`onerror`回调函数捕获。注意，这种错误无法通过状态码识别，因为HTTP回应的状态码有可能是200。

如果`Origin`指定的域名在许可范围内，服务器返回的响应，会多出几个头信息字段。

**（2）复杂请求（预检请求）**

复杂请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为**预检请求**，该请求必须首先使用 `option`方法发起一个预检请求到服务器，来知道服务端是否允许跨域请求。预检请求的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响。

满足以下任一条件，即可首先发送预检请求：

**1）使用以下任一HTTP方法：**

* `PUT`
* `DELETE`
* `CONNECT`
* `OPTIONS`
* `TRACE`
* `PATCH`

**2）人为设置了对CORS安全的首部字段集合之外的其他首部字段，该集合为：**

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Content-Type`
* `DPR`
* `Downlink`
* `Save-Data`
* `Viewport-Width`
* `Width`
* `Content-Type`：只限于三个值
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`
* 请求中的`XMLHttpRequestUpload`对象注册了任意多个事件监听器
* 请求中使用了`ReadableStream`对象

**2、JSONP**

**（1）JSONP是什么**

`JSONP`属于非同源策略（跨域请求），利用 `<script>`标签没有跨域限制的漏洞，网页可以得到从其他来源动态产生的 `JSON`数据。`JSONP`请求一定需要对方的服务器做支持才可以。

**（2）JSONP的优缺点**

* 优点：兼容性好，解决主流浏览器的跨域数据访问问题
* 缺点：仅支持`GET`方法，需要服务端配合，不安全，可能会遭受`XSS`攻击

**（3）JSONP的实现流程**

原生方式实现，创建`script`标签，再去请求一个带参网址来实现跨域通信：

```text
const jsonp = ({url, params, callback}) => {
    return new Promise((resolve, reject) => {
        let loaded = false;
        const script = document.createElement('script');
        window[callback] = data => {
            resolve(data);
            loaded = true;
            document.body.removeChild(script);
            window[callback] = null;
        };
        
        const p = {...params, callback};
        const array = [];
        for (let key in p) {
            array.push(`${key}=${[key]}`);
        }
        script.src = `${url}?${array.join("&")}`;
        document.body.appendChild(script);
        
        setTimeout(() => {
            loaded || reject("timeout");
        });
    });
};
jsonp({
    url: "http://www.sueRimn.com/jsonp",
    callback: "jsonpCallback"
});
```

`jQuery`也自带 `JSONP`请求方法，默认就会给JSONP的请求清除缓存：

```text
$.ajax({
    url: "http://www.sueRimn.com/jsonp",
    dataType: "jsonp",
    type: 'get',// 可省略
    jsonCallback: 'jsonCallback', //自定义传递给服务器的函数名，而不是使用jQuery自动生成的，可省略
    jsonp: 'callback', // 把传递函数名的那个形参callback，可省略
    success: data => {
        console.log(data);
    }
})
```

**（4）JSONP与CORS比较**

`ＣORS`比`JSONP`更强大。

`JSONP`只支持`GET`请求，`CORS`支持所有类型的`HTTP`请求。`JSONP`的优势在于兼容老式浏览器，以及可以向不支持`CORS`的网站请求数据。

**3、Websocket**

`Websocket`是`HTML5`的一个持久化的协议，它实现了浏览器与服务器的全双工通信，同时也是跨域的一种解决方案。`WebSocket`和`HTTP`都是应用层协议，都基于 `TCP`协议。

```text
// index.html
<script>
    let ws = new WebSocket('ws://localhost:9000');
    
    ws.onopen = function () {
      ws.send('hello from sueRimn');//向服务器发送数据
    }
    ws.onmessage = function (e) {
      console.log(e.data);//接收服务器返回的数据 hello from sever
    }
</script>
```

```text
//server.js
let Server = require('ws').Server;
const wss = new Server({
    port: 9001
})
wss.on('connection', ws => {
    ws.send('hello from sever');// 向客户端发送数据
    ws.on('message', data => {
        console.log(data); // 接收来自客户端的数据 hello from sueRimn
    }
})
```

**4、Node中间件代理**

**（1）方法一**

```text
// server.js
const express = require('express');
const cors = require('cors');
let app = express();
app.use("*", (req, res, next) => {
    res.header('Access-Control-Allow-Origin', '*'); // 表示任何域名都跨域访问，这样写不能携带cookie
    res.header('Access-Control-Allow-Headers', 'Content-Type, Content-Length, Authorization, Accept, X-Requested-With , yourHeaderFeild');
    res.header('Access-Control-Allow-Methods', 'PUT, POST, GET, DELETE, OPTIONS'); // 设置方法
    if (req.method === 'OPTIONS'){
        res.send(200); // 在正常请求之前， 会发送一个验证，是否可以请求
    } else {
        next();
    }
});
```

**（2）方法二**

直接安装`cors`

```text
npm install cors
```

然后再服务端这样写：

```text
// server.js
const express = require('express');
const cors = require('cors');
let app = express();
​
let corsOptions = {
    origin: 'http://www.baidu.com',
    optionsSuccessStatus: 200
}
​
app.get('/products/:id', cors(corsOptions), (req, res, next) => {
    res.json({msg: '只有百度才能访问'})
})
​
app.listen(80, () => {
    console.log('CORS-enabled web server listening on port 80')
})
```

**5、Nginx反向代理**

开发的过程中用中间人代理，而且都是不用入侵代码的做法，十分方便。而且现在有一种做法就是前端写 `node.js`和服务端渲染，所有请求交给 `node.js`一层，改善`SEO`问题和速度体验。

实现思路：通过`Nginx`配置一个代理服务器（域名与`domain1`相同，端口不同）做跳板机，反向代理访问`domain2`接口，并且可以顺便修改`cookie`中`domain`信息，方便当前域`cookie`写入，实现跨域登录。

**（1）下载Nginx**

[下载](http://nginx.org/en/download.html)，然后将解压在自己想要的文件夹，用`cmd`打开。

**注意，启动`nginx`不能直接双击`nginx.exe`，会改变配置文件`nginx.conf`，所以需要reload**

![](https://img-blog.csdnimg.cn/20190726170859708.png)

```text
// 启动nginx
start nginx
​
// 重启nginx
nginx -s reload
​
// 停止nginx
nginx -s stop
​
// 退出nginx
nginx -s quit
```

**（2）配置SSL**

打开`nginx`文件夹下的`nginx.conf`配置文件：

![](https://img-blog.csdnimg.cn/20190726170920833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1ZVJpbW4=,size_16,color_FFFFFF,t_70)

然后找到`https server`下的内容，取消注释，进行配置证书路径，这里写你证书所在路径即可：

![](https://img-blog.csdnimg.cn/20190726170939479.png)

打开你的`HOST`文件，一般是在这个路径`C:\Windows\System32\drivers\etc`：

![](https://img-blog.csdnimg.cn/20190726171025833.png)

打开之后，把我圈出的位置的注释取消掉，这里我已经去掉了：

![](https://img-blog.csdnimg.cn/20190726171036587.png)

然后你在浏览器中输入`https://localhost`出现下面这样的页面内容证明搭建成功啦：

![](https://img-blog.csdnimg.cn/20190726171050972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1ZVJpbW4=,size_16,color_FFFFFF,t_70)

**（3）反向代理Node服务**

**1）搭建Node服务器**

自己使用`express`或者`koa`搭建一个简单服务器即可，设置一个自定义的监听端口。我的服务器文件取名为`server.js`

然后使用`node server.js`启动服务确保能访问。

**2）Nginx反向代理配置**

再次来到`nginx.conf`配置文件，将`443`的`server location`里增加圈出的内容，端口号为自己在`Node`服务器设置的即可，`nginx`反向代理主要通过`proxy_pass`来配置：

![](https://img-blog.csdnimg.cn/2019072617111161.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1ZVJpbW4=,size_16,color_FFFFFF,t_70)

然后`cmd`输入`nginx -s reload`重启`nginx`，在浏览器输入`https:localhost`就可以访问到你项目的主页面了。如果在另一台局域网下的电脑浏览器中输入，就要输入成`https://你的局域网ip`即可访问一样的主页。

这是一个自签名证书，不能用于商用。

**6、window.name + iframe**

**7、location.hash + iframe**

**8、document.domain + iframe**

