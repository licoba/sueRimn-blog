# 理解类型操作符typeof

> 在本文中，将简述JavaScript类型系统和数据类型，以及如何使用`typeof`操作符执行类型检查。
>
> 还讲解了使用`typeof`操作符进行某些数据类型检查是不完善的，并介绍其他几种类型检查的方法。

每种编程语言都有自己的类型系统和数据类型，但各种编程语言的数据结构常有不同之处。使用`JavaScript`时，其引擎会在脚本执行期间隐式强制转换执行值的类型。类型检查对于编写可预测的`JavaScript`程序是非常有必要的。

> JavaScript中的`typeof`操作符就是用于基础的类型检查
>
> `typeof`操作符返回字符串，表示未经计算的操作数的类型（来自[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof)）

[![1568101627261](https://camo.githubusercontent.com/69a84447fa2c301fbcb4cd5fdddaa874acaed6f4/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662787837333f773d32373426683d3639)](https://camo.githubusercontent.com/69a84447fa2c301fbcb4cd5fdddaa874acaed6f4/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662787837333f773d32373426683d3639)

### 一、JavaScript的数据类型

在此之前，需要了解`JavaScript`有哪些数据类型，最新标准定义了8种数据类型：

* 7种原始类型：
  * `Boolean`
  * `Null`
  * `Undefined`
  * `Number`
  * `String`
  * `BigInt`
  * `Symbol`
* 其他为引用类型：
  * `Object`
  * 数组`array()`、函数`function()`、正则表达`RegExp`特殊的物体：
    * `array`是一种特殊的对象，它是一个有序的编号值集合
    * `function`是一种特殊类型的对象，具有与之关联的可执行脚本块有关

JavaScript还有几个对象类构造函数，用于创建其他类型的对象：

* `Date`—用于创建日期对象
* `RegExp`—用于创建正则表达式
* `Error`—用于创建`JavaScript`错误

### 二、`typeof`功能

#### 1、语法

`typeof`使用一元操作符（只需要一个操作数）的计算结果作为其操作数的类型结果字符串。

另一种替代语法就是将操作数放入括号中使用，这种对`JavaScript`表达式返回的值进行类型检查非常有用。

```text
typeof 参数
typeof(参数)
```

> 参数：一个表示对象或原始值的表达式，其类型将被返回

```text
typeof "sueRimn";        // 'string'
typeof 22;		        // 'number'
typeof NaN;              // 'number'
typeof Infinity;         // 'number'
typeof true;             // 'boolean'
typeof false;            // 'boolean'
typeof [1, 2];           // 'object'
typeof {age: 22};        // 'object'
typeof null;             // 'object'
typeof undefined;        // 'undefined' 
typeof String;           // 'function'
typeof Boolean;          // 'function' 
typeof Number;           // 'function'
typeof Object;           // 'function'
typeof Function;         // 'function'
typeof person;           // 'function'
```

> `undefined`是它自己的`JavaScript`类型

在ES6之前，`typeof`不管操作数是否声明，总是返回一个字符串，即对于未声明的标识符，总是返回而不是抛出错误。

在ES6中，使用`let`或`const`声明的块级作用域变量在初始化之前与`typeof`操作符使用，将抛出错误。

原因是：块级作用域变量在被初始化之前一直保留在临时死区。

```text
console.log(typeof name === 'undefined'); // ReferenceError
const name = 'sueRimn';
```

#### 2、一般类型检查

在`JavaScript`中执行类型检查主要使用`typeof`操作符

```text
function add(a, b) {
    if (typeof a !== 'number' || typeof b !== 'number') {
        throw '参数必须是数值'
    }
    return a + b;
}
```

以下是对类型检查的简单摘要：

| 值 | 类型 |
| :--- | :--- |
| `undefined` | `"undefined"` |
| `null` | `"object"` |
| 原始对象 | `"object"` |
| 所有数组 | `"object"` |
| 其他对象 | `"object"` |
| `true`/`false` | `"boolean"` |
| 字符串 | `"string"` |
| 所有符号 | `"symbol"` |
| 所有函数 | `"function"` |
| 宿主对象 | 依赖于实现 |

检测值是否存在在不同环境是这样的：

```text
if (typeof window !== 'undefined') {
  // 浏览器
};

if (typeof process !== 'undefined') {
  // Node.js
}

if (typeof $ !== 'undefined') {
  // jQuery 
}
```

#### 3、其他类型检查

对于某些值需要额外的类型检查才可以区分，例如`null`和`[]`在使用`typeof`操作符执行type-check时都是`"object"`类型，但是区分它们需要额外的操作。

**一些其他数据类型值检查方法**：

* 使用`instanceof`
* 检查对象的`constructor`属性
* 使用对象的`toString()`方法检查对象类

**（1）检测是否为空**

使用`typeof`操作符检查值是否为空并不好，检查值是否为空的最佳方法是对值与关键字进行严格相等比较。

[![1568105084389](https://camo.githubusercontent.com/92d27d02366b3a6a93803492b3d125b15cc70579/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662787837373f773d32343726683d3735)](https://camo.githubusercontent.com/92d27d02366b3a6a93803492b3d125b15cc70579/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662787837373f773d32343726683d3735)

以上代码呈现的结果是不一样的，所以使用严格相等操作符是非常重要的。

**（2）检测NaN**

> 任何涉及`NaN`的算术运算都将对`NaN`求值，如果想为任何形式的算术运算使用值，那么需要确保该值不是`NaN`。

使用`typeof`操作符检查`NaN`值是否返回`"number"`。要检查`NaN`值，可以使用全局函数`isNaN()`，或者ES6中的`Number.isNaN()`函数:

[![1568105282289](https://camo.githubusercontent.com/8a622f60f17311220aad5798567bef2b489d5a80/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662787838673f773d33313826683d323839)](https://camo.githubusercontent.com/8a622f60f17311220aad5798567bef2b489d5a80/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662787838673f773d33313826683d323839)

`NaN`值非常特殊，通过比较，它永远不等于任何其他值，包括它自己:

[![1568106586133](https://camo.githubusercontent.com/a86c67e83d5b4b49be6ae1574a66e8771e915653/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662787838683f773d32313026683d3834)](https://camo.githubusercontent.com/a86c67e83d5b4b49be6ae1574a66e8771e915653/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662787838683f773d32313026683d3834)

可以使用以下方法在非ES6环境下检测`NaN`：

```text
function isNan(value) {
  return value !== value;
}
```

最后，你可以利用ES6中的`Object.is()`函数来测试值是否为`NaN`。

以下函数作用是检查两个值是否相同：

```text
function isNan(value) {
  return Object.is(value, Number.NaN);
}
```

**（3）检测数组**

使用`typeof`检查数组将返回`object`。这里介绍几种检测数组的方法，并进行对比：

* 使用`constructor`属性（不推荐）

```text
function isArray(value) {
  return typeof value == 'object' && value.constructor === Array;
}
```

* 使用`instanceof`（不推荐，因为对象的原型可以更改）

```text
function isArray(value) {
  return value instanceof Array;
}
```

* 使用`Object.prototype.toString()`（推荐，类似于`ES6 Array.isArray()`）

```text
function isArray(value) {
  return Object.prototype.toString.call(value) === '[object Array]';
}
```

> `object.prototype.tostring()`方法对于检查任何`JavaScript`值的对象类型都非常有用

* 使用`ES6 Array.isArray()`

```text
function isArray(value) {
  return Array.isArray(value);
}
```

