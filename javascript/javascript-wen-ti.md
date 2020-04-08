# JavaScript问题

1、undefined与null的区别

`undefined`和`null`都代表`false`值，但是它们有区别：

* `undefined`是未指定特定值的变量、没有显性返回值的函数或者对象中不存在的属性的默认值，`JavaScript`引擎会自动分配`undefined`值

```text
let name; 
const person = () => {} 
const my = {
    name: 'sueRimn',
}
console.log(name) // logs undefined
console.log(person) // logs undefined
console.log(someObj['age']) // logs undefined
​
```

* `null`的意思是没有值，一般是显示的定为变量的值
* 当使用`==`比较`undefined`和`null`时，返回`true`，当使用`===`比较两者时，返回`false`

#### 2、什么是事件传播

当`DOM`元素发生事件时，该事件并不一定发生在该元素上，在冒泡阶段，该事件冒泡，传到其父元素、祖父元素或曾祖父元素，直到到达`window`为止，而在捕获阶段，事件从`window`开始，一直到触发事件或`event.target`的元素。

事件传播有三个阶段：

* 捕获阶段——事件从`window`开始从上往下穿过每个元素，直到到达目标元素
* 目标阶段——事件已到达目标元素
* 冒泡阶段——事件从开始元素从下往上冒泡直到到达`window`

#### 3、`event.preventDefault()`和`event.stopPropagation()`的区别是什么

* `event.preventDefault()`方法阻止一个元素的默认行为。如果在表单中使用，则会阻止它提交。如果在`anchor`元素中使用，则会阻止它导航。如果在`contextmenu`中使用，则阻止它显示。
* `event.stopPropagation()`方法阻止一个事件的传播或者阻止事件在冒泡或者捕获阶段发生

#### 4、如何知道一个元素使用了`event.preventDefault()`方法

可以在事件对象中使用`event. defaultprevent`属性，它会返回一个布尔值，指示是否在特定元素中调用了`event.preventDefault()`

#### 5、为什么`obj.someprop.x`会报错

```text
const obj = {}
console.log(obj.someprop.x) 
```

很显然会抛错，因为当试图访问`someprop`属性中的`x`属性时，它有一个未定义的值。对象中的属性本身并不存在，其原型的默认值为`undefined`，`undefined`没有属性`x`。

#### 6、`event.currentTarget`是什么

`event.currentTarget`是显式附加事件处理程序的元素。

#### 7、绝对相等`==`和严格相等`===`的区别

`==`在强制转换后通过值进行比较，而`===`在没有强制转换的情况下通过值和类型进行比较。

强制转换是将一个值转换为另一种类型的过程。`==`执行隐式强制转换，在比较两个值之前，`==`要执行一些条件

#### 8、为什么比较两个相似对象会返回`false`

