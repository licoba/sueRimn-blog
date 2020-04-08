# DOM事件机制

### 一、事件机制

事件是在编程时系统内发生的动作或者发生的事情，系统会在事件出现的时候触发某种信号并且会提供一个自动加载某种动作的机制（来自MDN）。

每个事件都有事件处理器（有时也叫事件监听器），也就是触发事件时运行的代码块。严格来说事件监听器监听事件是否发生，然后事件处理器对事件做出反应。

### 二、DOM事件流

事件传播是一种机制，用于定义事件如何传播或通过DOM树传播，事件传播有两种方式：事件捕获\(Capture\)和事件冒泡\(Bubble\)。

事件传播形式上有三个阶段：

* 捕获阶段：从窗口进入事件目标阶段
* 目标阶段： 目标阶段
* 冒泡阶段：从事件目标回到窗口

但是，目标极端在现代浏览器中没有单独处理，所以当一个事件发生在具有父元素的元素上时，现代浏览器运行两个不同的阶段 - 捕获阶段和冒泡阶段。

[![](https://camo.githubusercontent.com/3cebd661bcd159bbbe9e6db44f9556cc786630d8/68747470733a2f2f7777772e7475746f7269616c72657075626c69632e636f6d2f6c69622f696d616765732f6576656e742d70726f7061676174696f6e2d696c6c757374726174696f6e2e706e67)](https://camo.githubusercontent.com/3cebd661bcd159bbbe9e6db44f9556cc786630d8/68747470733a2f2f7777772e7475746f7269616c72657075626c69632e636f6d2f6c69622f696d616765732f6576656e742d70726f7061676174696f6e2d696c6c757374726174696f6e2e706e67)

如上图演示了在具有父元素的元素上触发事件时，事件在DOM树中层层往下进行事件传播。

引入事件传播的概念是为了当具有父子关系的DOM层次结构中的多个元素具有针对同一事件的事件处理程序（例如鼠标单击），应该怎样处理。

### 三、事件捕获

事件发生时，在捕获阶段，事件从窗口向下通过DOM树传播到目标节点，即从最外层元素（祖先元素）触发事件响应函数，逐级往下，直到目标元素。

如果目标元素的任何祖先\(即父、祖父等\)和目标本身具有针对该类型事件专门注册的捕获事件侦听器，则这些侦听器将在捕获阶段执行。

```text
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>JavaScript Event Capturing Demo</title>
<style type="text/css">
    div, p, a{
        padding: 15px 30px;
        display: block;
        border: 2px solid #000;
        background: #fff;
    }
</style>
</head>
<body>
<div id="wrap">DIV
    <p class="hint">P
        <a href="#">A</a>
    </p>
</div>

<script>
    function showTagName() {
        alert("Capturing: "+ this.tagName);
        console.log()
    }
    
    var elems = document.querySelectorAll("div, p, a");
    for(let elem of elems) {
        elem.addEventListener("click", showTagName, true);
    }
</script>
</body>
</html>  
```

[![](https://camo.githubusercontent.com/cfcb6e2113d5b760b9e6ff0a25dd5444abc18e2f/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662794831573f773d32353626683d323035)](https://camo.githubusercontent.com/cfcb6e2113d5b760b9e6ff0a25dd5444abc18e2f/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662794831573f773d32353626683d323035)

其触发结果是：

```text
// 点击DIV时
Capturing: DIV

// 点击P时
Capturing: DIV
Capturing: P

// 点击a时
Capturing: DIV
Capturing: P
Capturing: A
```

可以看出，触发具有父元素的元素的事件时，都是从最外层开始逐级往下传播事件的。

在捕获阶段用到的事件监听器是：

```text
target.addEventListener(type, listener, useCapture(optional));
```

* `target`： 事件目标
* `type`：事件类型
* `listener`：事件触发响应函数
* `useCapture(optional)`：一个布尔值，指示在将该类型的事件分配给DOM树中它下面的任何`EventTarget`之前，是否将其分配给注册的侦听器，捕获阶段为`true`，没有值时默认为`false`

### 四、事件冒泡

在事件冒泡阶段，正好相反。

事件冒泡模式流程：事件发生时，先触发目标元素（最直接元素）的事件响应函数，然后触发其父元素的事件响应函数，并逐级上溯到祖先元素。

**在现代浏览器中，默认所有事件处理程序都注册在冒泡阶段**。

```text
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>JavaScript Event Bubbling Demo</title>
<style type="text/css">
    div, p, a{
        padding: 15px 30px;
        display: block;
        border: 2px solid #000;
        background: #fff;
    }
</style>
</head>
<body>
<div onclick="alert('Bubbling: ' + this.tagName)">DIV
    <p onclick="alert('Bubbling: ' + this.tagName)">P
        <a href="#" onclick="alert('Bubbling: ' + this.tagName)">A</a>
    </p>
</div>
</body>
</html>  
```

[![](https://camo.githubusercontent.com/cfcb6e2113d5b760b9e6ff0a25dd5444abc18e2f/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662794831573f773d32353626683d323035)](https://camo.githubusercontent.com/cfcb6e2113d5b760b9e6ff0a25dd5444abc18e2f/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662794831573f773d32353626683d323035)

其触发结果是：

```text
// 点击DIV时
Capturing: DIV

// 点击P时
Capturing: P
Capturing: DIV

// 点击a时
Capturing: A
Capturing: P
Capturing: DIV
```

所有浏览器都支持事件冒泡，它适用于所有处理程序，不管它们是如何注册的，例如使用`onclick`或`addEventListener()`\(除非它被注册为捕获事件监听器\)。这就是为什么事件传播这个术语经常被用作事件冒泡的同义词。

### 五、访问目标元素

目标元素是生成事件的DOM节点。

`event.target`可以作为目标元素访问，其不会在事件传播阶段改变。

此外，`this`关键字表示当前元素\(即具有当前正在运行的处理程序的元素\)。

### 六、阻止事件传播

在嵌套的元素中，并且每个元素都有事件处理程序时，当单击内部元素，所有处理程序都将同时执行，因为事件会出现在DOM树中。

**为了防止这种情况，可以使用`event.stopPropagation()`方法停止事件使DOM树冒泡。**

在以下示例中，如果单击子元素，则不会执行父元素上的`click`事件监听器。

```text
<div id="wrap">DIV
    <p class="hint">P
        <a href="#">A</a>
    </p>
</div>

<script>
    function showAlert(event) {
        alert("You clicked: "+ this.tagName);
        event.stopPropagation();
    }

    var elems = document.querySelectorAll("div, p, a");
    for(let elem of elems) {
        elem.addEventListener("click", showAlert);
    }
</script>
```

另外，甚至可以使用`stopImmediatePropagation()`方法阻止执行附加到同一事件类型的相同元素的其他任何侦听器。

```text
<div onclick="alert('You clicked: ' + this.tagName)">DIV
  <p onclick="alert('You clicked: ' + this.tagName)">P
      <a href="#" id="link">A</a>
  </p>
</div>

<script>
  function sayHi() {
      alert("Hi, sueRimn!");
      event.stopImmediatePropagation();
  }
  function sayHello(){
      alert("Hello World!");
  }
  
  // 将多个事件处理程序附加到超链接
  var link = document.getElementById("link");
  link.addEventListener("click", sayHi);  
  link.addEventListener("click", sayHello);
</script>
```

如果将多个侦听器附加到同一事件类型的同一元素上，则按它们被添加的顺序执行。但是，如果任何侦听器调用\`\`event.stopImmediatePropagation\(\) ``方法，则不会执行剩余其他侦听器。

比如上方代码，按照添加顺序，应该先执行`sayHi()`，然后执行`sayHello()`，但是在执行`sayHi()`时调用了`event.stopImmediatePropagation()` 方法，所以`sayHello()`不会被执行了。

### 七、阻止默认事件

有些事件具有与之关联的默认操作。例如点击一个链接浏览器带你到链接的目标，点击一个表单提交按钮浏览器提交表单等等。

可以使用事件对象的`preventDefault()`方法来防止此类默认操作。但是，阻止默认操作并不会停止事件传播，事件像往常一样继续传播到DOM树。

### 八、小结

目前主流的浏览器事件传播流程都遵循DOM的事件流机制，即先由外到内捕获流程，然后是由内到外的冒泡流程。大多数情况下，都只在冒泡阶段响应事件，捕获事件很少使用。虽然\`\`stopPropagation\`可以阻断事件流，但是通常不推荐使用，只有在确实有必要时使用。

> 参考：  
> [Event Bubbling and Event Capturing In JavaScript: All you need to know](https://www.edureka.co/blog/event-bubbling-and-event-capturing/)  
> [JavaScript Event Propagation](https://www.tutorialrepublic.com/javascript-tutorial/javascript-event-propagation.php)

