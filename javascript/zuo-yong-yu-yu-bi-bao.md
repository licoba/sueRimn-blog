# 作用域与闭包

### 一、作用域

作用域是追踪所有变量的方式，是代码的当前上下文以及对变量的访问权限。了解作用域，可以知道变量/函数在何处可访问。

`JavaScript`使用**词法作用域**，这种方法允许作用域嵌套，因此外部作用域包含内部作用域。

#### 1、全局作用域

如果一个变量在所有函数或花括号\({}\)之外声明，则它是在全局作用域内定义的

全局变量可以在代码的任何地方使用。

```text
const name = 'sueRimn';

function person () {
    console.log(name);
}

console.log(name); // 'sueRimn'
person() // 'sueRimn'
```

虽然可以在全局范围内声明变量，但不建议这样做，因为存在命名冲突的可能性。

如果使用`const`或`let`声明变量，那么每当发生名称冲突时，都会抛错。这是不可取的。

```text
let name = 'sueRimn';
let name = '八至'; // 报错
```

如果使用`var`声明变量，第二个变量会在声明后覆盖第一个变量。这也不可取，因为代码将很难调试。

```text
var name = 'sueRimn';
var name = '八至';
console.log(name); // '八至'
```

所以，你应该声明局部变量，而不是全局变量。

> 这只适用于web浏览器中的JavaScript。

#### 2、局部作用域

只在代码的特定部分中可用的变量被认为是在局部作用域中。这些变量也称为局部变量。

**在`JavaScript`中，有两种局部作用域：函数作用域和块作用域**。

#### （1）块作用域

当在一个大括号\({}\)内声明一个`const`或`let`变量时，只能在那个大括号内访问这个变量。

```text
{
    let name = 'sueRimn';
    console.log(name); // 'sueRimn'
}

console.log(name); // error, name is not defined
```

块作用域是函数作用域的一个子集，因为函数需要用花括号声明\(除非使用带隐式返回的箭头函数\)。

#### （2）函数作用域

在函数中声明变量时，只能在函数中访问该变量，对变量的访问仅限于函数的局部作用域。

```text
function person () {
    let name = 'sueRimn';
    console.log(name); 
}

person(); // 'sueRimn'
console.log(name); // 报错 name is not defined
```

**a）函数提升与作用域**

当使用函数声明声明函数时，总是将其提升到当前范围的顶部，以下两种结果是一样的：

```text
person(); // 'sueRimn is beautiful'

function person () {
    console.log('sueRimn is beautiful');
}

person(); // 'sueRimn is beautiful'
```

当使用函数表达式表明时，函数不会提升到当前范围的顶部。

```text
person(); // 报错 person is not defined
const person = () =>{
    console.log('sueRimn is beautiful');
}

person(); // 'sueRimn is beautiful'
```

所以，尽量在使用函数之前声明它。

**b）独立函数不能访问彼此的作用域**

如果分别独立声明函数，即使函数之间可以彼此调用，但是无法访问彼此的变量，因为每个函数的作用域是独立的。

```text
function name () {
    const name = 'sueRimn';
}

function age () {
    const age = '22'
    name()
    console.log(name); // error name id not defined.
}
```

**c）嵌套作用域**

当在一个函数中定义另一个函数时\*\*，内部函数可以访问外部函数的作用域\*\*。函数嵌套也会导致作用域嵌套，作用域嵌套也称为**词法作用域**或**闭包**，也成为**静态作用域**。

但是，**外部函数无法访问内部函数的作用域**。就像单向玻璃，你在里面可以看见外面，外面的看不见里面。

[![](https://camo.githubusercontent.com/d8d10c9d57eb2c15a47727bc094d03c205e842f2/68747470733a2f2f7265732e636c6f7564696e6172792e636f6d2f6373732d747269636b732f696d6167652f75706c6f61642f635f7363616c652c775f313030302c665f6175746f2c715f6175746f2f76313530323438323437392f6f6e652d7761792d676c6173735f6c35646732672e706e67)](https://camo.githubusercontent.com/d8d10c9d57eb2c15a47727bc094d03c205e842f2/68747470733a2f2f7265732e636c6f7564696e6172792e636f6d2f6373732d747269636b732f696d6167652f75706c6f61642f635f7363616c652c775f313030302c665f6175746f2c715f6175746f2f76313530323438323437392f6f6e652d7761792d676c6173735f6c35646732672e706e67)

```text
function person () {
    let name = 'sueRimn';
    function my () {
        console.log('my name is' + name);
    }
    console.log(name);
    my();
}

// 打印结果是：
'sueRimn' 
'my name is sueRimn'
```

#### 3、作用域链

**（1）作用域与执行上下文**

JavaScript属于解释型语言，JavaScript的执行分为解释和执行两个阶段：

解释阶段：

* 词法分析
* 语法分析
* 作用域规则确定

执行阶段：

* 创建执行上下文
* 执行函数代码
* 垃圾回收

静态作用域是指函数定义决定了函数的作用域。JavaScript采用的是静态作用域。JavaScript解释阶段便会确定作用域规则，因此作用域在函数定义时就已经确定了，而不是在函数调用时确定。

执行上下文是函数执行之前创建的，即在函数执行准备阶段创建好的。

执行上下文最明显的就是this的指向是执行时确定的，即函数调用决定执行上下文的指向。

**（2）深入理解作用域链**

**a）定义**

因为 `JavaScript` 采用的是词法作用域（静态作用域），函数定义时确定自己的作用域作为该函数的属性，作用域无法改变，一直保存至函数销毁。

所以说函数定义时是基于静态作用域的，因为即使函数不调用，其\[\[scope\]\]属性也会一直存在，并且保持不变。

每个上下文都有自己的变量对象，对于全局上下文，它是全局对象自身；对于函数，它是活动对象。

当查找变量对象时，计算机会从当前上下文的变量对象中找，如果找不到，就会从父级上下文也就是层层往上查找，直到全局上下文，到那时还找不到，就会抛出`ReferenceError`。

**作用域链正是内部上下文所有变量对象的链表**，用于变量查询。

函数上下文的作用域链在函数调用时创建的，包含活动对象和这个函数内部的`[[scope]]`属性。

因为当函数调用时，会生成执行上下文，此执行上下文的`[[scope]]`和定义函数时的`[[scope]]`是不同的，执行上下文的`[[scope]]`是在函数定义时的`[[scope]]`属性基础上又新增一个当前AO对象构成的。

因此，函数定义时候的`[[scope]]`作为函数的属性，函数执行时候的`[[scope]]`作为函数执行上下文的属性。

一般情况下，一个作用域链包括父级变量对象（variable object）（作用域链的顶部）、函数自身变量VO和活动对象（activation object）。

当查找标识符的时候，会从作用域链的活动对象部分开始查找，然后\(如果标识符没有在活动对象中找到\)查找作用域链的顶部，循环往复，就像作用域链那样。

标识符解析过程与函数声明周期相关。

**b）了解函数的声明周期**

函数周期分为函数创建和函数调用

**函数创建**

在进入上下文时函数声明放到变量/活动（VO/AO）对象中。

**函数调用**

进入上下文创建AO/VO之后，上下文的Scope属性（变量查找的一个作用域链）作如下定义：

```text
Scope = AO|VO + [[Scope]]
```

一个函数对象被调用的时候，会创建一个**活动对象\(也就是一个对象\)**，对于每一个函数的形参，都命名为该活动对象的命名属性，然后将这个活动对象作为此时的作用域链最前端，并将这个函数对象的\[\[scope\]\]加入到作用域链中。

### 二、闭包

#### 1、定义

闭包与词法作用域直接相关，函数创建时存储作用域，直到到函数销毁都不会改变。

实际上，闭包是由函数以及创建该函数的词法环境组合而成。**这个环境包含了这个闭包创建时所能访问的所有局部变量**。

闭包允许你从内部函数访问外部函数的作用域。在JavaScript中，每次在函数调用时都会创建闭包。

* 闭包是嵌套函数，可以访问外部范围
* 返回外部函数后，通过对内部函数（闭包）的引用，可以防止破坏外部作用域
* 如果外部作用域的变量被更改，它将影响后续调用

#### 2、使用闭包

要使用闭包，就要在一个函数中定义另一个函数并暴露该内部函数。若要公开一个内部函数，就要将其返回或传递给另一个函数。即使被外部函数返回之后，内部函数也可以访问外部函数作用域中的变量。

**（1）使用闭包保护私有数据**

在JavaScript中，闭包是用来保护数据隐私的主要机制。闭包是外部范围和程序其余部分之间的通道。它可以选择公开什么数据，而不公开什么数据。

```text
function person() {
    let age = 22; 
    return { 
        getAge: function() {
            return age;
        },
        setAge: function(v) {
            age = v;
        }
    };
}

obj = person();

console.log(obj.getAge()); // 22

obj.setAge(22);
console.log(obj.getAge()); // 22

obj.setAge("sueRimn");
console.log(obj.getAge()); // sueRimn
```

这里函数返回了一个有两个函数的对象。因为它们是绑定到局部作用域的对象的属性，所以它们是闭包。通过`getAge`和`setAge`，可以操作`age`属性，但不能直接访问它。

对象不是产生数据隐私的唯一方法。闭包也可以用来创建有状态函数，这些函数的返回值可能会受到其内部状态的影响，比如：

```text
const name = name => () => name;
```

**（2）使用闭包创建迭代器**

由于保存了来自外部作用域的数据，所以使用闭包创建迭代器相当容易。

```text
function buildContor(i) { 
    var contor = i;
    var displayContor = function() {
        console.log(contor++);
        contor++;
    };
    return displayContor; 
}

var myContor = buildContor(1);
myContor(); // 1
myContor(); // 2
myContor(); // 3

// new closure - new outer scope - new contor variable
var myOtherContor = buildContor(10);
myOtherContor(); // 10 
myOtherContor(); // 11

// myContor was not affected 
myContor(); // 4
```

上面的`buildContor()`函数实际上是一个迭代器，每次调用都创建一个新的迭代器，并使用固定的起始索引，然后在每次连续调用迭代器时，返回下一个值。

每次调用其中一个计数器时，通过改变这个变量的值，会改变这个闭包的词法环境。然而在一个闭包内对变量的修改，不会影响到另外一个闭包中的变量。

**（3）使用jQuery时的闭包**

jQuery\(或任何JavaScript\)中的事件都是闭包。事件处理程序可以访问外部作用域。

```text
$(function() { 
    var contor = 0;
    $("#Button").click(function() { // 闭包从外部作用域更新变量
        contor++; 
    }
}
```

**（4）使用闭包在JavaScript中实现单例**

单例对象是在程序执行过程中只有一个实例的对象。

我们知道，每次函数调用都会创建一个新的闭包。但如果我们想阻止外部函数的另一次调用呢?

很简单：使用匿名函数。

```text
var person = function () {
    var age = 22;
    return {
        get: function () {
            return "age: " + age;
        },
        increment: function() {
            age++;
        }
    };
}();  // 注意 单例是该函数回调的结果

console.log(person.get()); // age:22
console.log(person.get()); // age:22

person.increment();
console.log(person.get()); // age:23
person.increment();
console.log(person.get()); // age: 24
```

这个例子与前面唯一的区别是外部函数是匿名的，它没有名字。

我们声明它并立即调用它，`person`对象\(即闭包\)是访问其作用域的唯一来源。对于确保创建的`age`不会有多个作用域是非常有用的。

#### 3、性能考量

如果不是某些特定任务需要使用闭包，在其它函数中创建函数是不明智的，因为闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

### 三、小结

作用域和闭包如果从单向玻璃理解就很容易。

作用域是在函数定义时产生的，在一个函数内定义任何内部函数，其内部函数称为闭包，闭包保留对外部函数中创建的变量的访问权。

> 参考：
>
> [Master the JavaScript Interview: What is a Closure?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36)
>
> [Closures in Javascript for beginners](https://www.codingame.com/playgrounds/6516/closures-in-javascript-for-beginners)
>
> [JavaScript Scope and Closures](https://css-tricks.com/javascript-scope-closures/)
>
> [Closures](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)

