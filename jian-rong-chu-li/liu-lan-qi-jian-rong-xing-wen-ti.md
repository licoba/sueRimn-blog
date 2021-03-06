# 浏览器兼容性问题

### 常用浏览器兼容

**前缀兼容**

 \| Chrome \| Firefox \| Safari \| IE \| Opera ------ \| ------- \| ------- \| ------- \| ------ 渲染引擎 \| Blink \| Gecko \| Webkit \| Trident \| Blink JS引擎 \| V8 \| SpiderMonkey \| Nitro \| Chakra \| V8 前缀 \| -webkit\| -moz \| -webkit \| -ms \| -o

**透明属性opacity**

 透明属性opacity,解决IE9以下浏览器不能使用opacity问题：

```text
 opacity:0.5;
 filter:alpha(opacity = 50);//IE6-IE9习惯使用filter属性来 进行实习
 filter:progrid:DXImageTransform.Microsoft.Alpha(style = 0,opacity = 50);//IE4-IE9都支持
```

**浏览器 hack**

1.判断IE浏览器

```text
 <!--[if IE ]> ie8 <![endif]-->
​
```

2.判断Safari浏览器

```text
 let isSafari = /a/.__proto__=='//';
​
```

3.判断Chrome浏览器

```text
 let isChrome = Boolean(window.chrome);
​
```

### 样式兼容性\(css\)

**使用Normalize.css**

 不同浏览器样式存在差异，可以使用这个去除差异

**使用通配符选择器，全局重置样式**

 不加样式控制，不同浏览器外补丁和内补丁不同，用以下方法：

```text
 * {
  margin:0;
  padding:0;
 }
```

**display:inline转为行内属性**

 块属性标签设置浮动`float`后，又有横行的`margin`情况下，在`IE6`显示`margin`比设置的大。

 解决方案：在`float`的标签样式控制中加入 `display:inline`,将其转化为行内属性

**图片间距设置font-size:0**

 图片默认有间距，设置font-size

### 交互兼容性（JavaScript）

 1.封装适配器进行事件兼容，过滤事件句柄绑定、移除、冒泡阻止以及默认事件行为处理

```text
  var  helper = {}
​
 //绑定事件
 helper.on = function(target, type, handler) {
    if(target.addEventListener) {
        target.addEventListener(type, handler, false);
    } else {
        target.attachEvent("on" + type,
            function(event) {
                return handler.call(target, event);
            }, false);
    }
 };
​
 //取消事件监听
 helper.remove = function(target, type, handler) {
    if(target.removeEventListener) {
        target.removeEventListener(type, handler);
    } else {
        target.detachEvent("on" + type,
        function(event) {
            return handler.call(target, event);
        }, true);
     }
 };
​
```

 2.使用`new Date(str)`来正确生成日期对象-&gt;'`2018/07/05`'。 3.获取`scrollTop`通过`document.documentElement.scrollTop`兼容非`Chrome`浏览器

```text
  var scrollTop = document.documentElement.scrollTop||document.body.scrollTop;
​
```

### 参考阅读

 [如何机智的回答浏览器兼容问题](https://juejin.im/post/5b3da006e51d4518f140edb2)

