# TypeScript

Typescript

### 一、什么是typescript

#### 1、typescript

`Typescript`是`JavaScript`的一个超集，主要提供了**类型系统**和**对 ES6 的支持**。

优点：

* TypeScript 增加了代码的可读性和可维护性
* 非常包容

### 2、typescript与JavaScript的区别

### 二、指定变量类型

使用`:`指定变量的类型，`:` 的前后有没有空格都可以

### 三、基本数据类型

基础数据类型包括：布尔值\(boolean\)、数值\(`number`\)、字符串\(string\)、`null`、`undefined` 、`Symbol`、任意值（`any`）、`Object`、`Never`、`viod`、枚举等，其中分为原始数据类型、非原始数据类型。

#### 1、原始数据类型

原始数据类型包括：布尔值\(boolean\)、数值\(`number`\)、字符串\(string\)、`null`、`undefined` 、`Symbol`

**（1）布尔值**

```text
let isDone: boolean = false;
```

使用构造函数 `Boolean` 创造的对象**不是**布尔值，是布尔对象。

**（2）数值**

```text
let one: number = 6;
let two: number = 0xf00d;
let three: number = NaN;
let four: number = Infinity;
```

**（3）字符串**

```text
let name: string = 'sueRimn';
​
// 模板字符串
let str: string = `hellow, ${name}`.
```

进一步说一下字符串字面量类型。

**字符串字面类型**

字符串字面量类型用来约束取值只能是某几个字符串中的一个。

注意，**类型别名与字符串字面量类型都是使用** `type` **进行定义。**

```text
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent (ele: Element, event: EventNames) {
    
}
​
handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dbclick'); // 报错，event 不能为 'dbclick'
​
// index.ts(7,47): error TS2345: Argument of type '"dbclick"' is not assignable to parameter of type 'EventNames'.
```

使用 `type` 定了一个字符串字面量类型 `EventNames`，它只能取三种字符串中的一种。

**（4）空值**

JavaScript 没有空值（`Void`）的概念，在 TypeScript 中，可以用 `void` 表示没有任何返回值的函数

```text
function alterName(): void {
    alert('my name is Tom')
}
```

声明一个 `void` 类型的变量没有什么用，因为你只能将它赋值为 `undefined` 和 `null`：

```text
let unusable: void = undefined;
```

**（5）Null和Undefined**

与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量，而 `void` 类型的变量不能赋值给 `number` 类型的变量。

```text
let one: undefined = undefined;
// let one: undefined;
let two: null = null;
```

#### 2、其他数据类型

**（1）数组类型**

**定义数组**

两种方式定义数组：

* 元素类型后面加`[]`

```text
let list: number[] = [1, 2, 3];
```

* 使用数组泛型，`Array<元素类型>`：

```text
let list: Array<number> = [1, 2, 3];
```

**接口表示数组（不推荐）**

```text
interface NumberArr {
    [index: number]: number
}
let arr: NumberArr = [11, 2, 3, 4]
```

`NumberArr`表示：只要索引的类型是数字时，那么值的类型必须是数字

**类数组**

类数组（Array-like Object）不是数组类型，不能用普通的数组的方式来描述，而应该用接口：

```text
function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments;
}
```

除了约束当索引的类型是数字时，值的类型必须是数字之外，也约束了它还有 `length` 和 `callee` 两个属性。

**（2）元组类型**

元组类型允许各元素的类型不必相同。规定元组元素类型的时候，其值不能乱顺序：

```text
let x: [string, number];
x = ['sueRimn', 22]; // correct
x = [22, 'sueRimn']; // error
```

**（3）枚举类型**

枚举（`Enum`）类型用于取值被限定在一定范围内的场景。

```text
enum Color {red = 1, yellow, blue}
let c: Color = Color.yellow;
```

默认情况下，从`0`开始为元素编号。 手动的指定成员的数值：

```text
enum Color {red, yellow, blue}
let c: Color = Color.yellow;
```

**a）常数项和计算所得项**

枚举项有两种类型：常数项（constant member）和计算所得项（computed member）。

一个典型的计算所得项的例子：

```text
enum Color {Red, Green, Blue = "blue".length};
```

`"blue".length` 就是一个计算所得项。计算所得项后续的项不能为未手动赋值的项，不热就会报错。

```text
enum Color {Red = "red".length, Green, Blue};
​
// index.ts(1,33): error TS1061: Enum member must have initializer.
// index.ts(1,40): error TS1061: Enum member must have initializer.
```

**b）常数枚举**

常数枚举是使用 `const enum` 定义的枚举类型：

```text
const enum Directions {
    Up,
    Down,
    Left,
    Right
}
​
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

常数枚举与普通枚举的区别是，它会在编译阶段被删除，并且不能包含计算成员。

**c）外部枚举**

外部枚举（Ambient Enums）是使用 `declare enum` 定义的枚举类型，`declare` 定义的类型只会用于编译时的检查，编译结果中会被删除。

外部枚举与声明语句一样，常出现在声明文件中。

**（4）函数类型**

**函数声明**

一个函数有输入和输出，要在 TypeScript 中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义较简单：

```text
function sum(x: number, y: number): number {
    return x + y;
}
```

注意，**输入多余的（或者少于要求的）参数，是不被允许的**

**函数表达式**

```text
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

在 TypeScript 的类型定义中，`=>` 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。

**接口定义函数的形状**

```text
interface SearchFunc {
    (source: string, subString: string): boolean;
}
​
let mySearch: searchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

**可选参数**

用`?`表示可选参数。可选参数后面不允许再出现必需参数，可选参数必须接在必需参数后面。

```text
function person(firstNameL string, lastName?: string) {
    if (lastName) {
        return firstName + '' + lastName;
    } else {
        return firstName;
    }
}
let my = person('sueRimn', 'wang');
let he = person('tao');
```

**参数默认值**

**TypeScript 会将添加了默认值的参数识别为可选参数**：

```text
function person (name: string, age: number = 23) {
    return name + ',' + age
}
let my = person('sueRimn', 23)
let other = person('八至')
```

不受可选参数必须在必需参数后面的限制。

**剩余参数**

必要参数，默认参数和可选参数有个共同点：它们表示某一个参数。 有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来，在`TypeScript`里，你可以把所有参数收集到一个变量里

```text
function people (array: any[], ...item: any[]) {
    item.forEach(function(item) {
        array.push(item)
    })
}
```

**重载**

重载允许一个函数接受不同数量或者类型的参数时，做出不同的处理。

**（5）Object对象类型**

`object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。

**（6）任意值类型**

如果是一个普通类型，在赋值过程中改变类型是不被允许的：

```text
let name: string = 'sueRimn';
name = 22; // Type 'number' is not assignable to type 'string'.
```

但如果是 `any` 类型，则允许被赋值为任意类型：

```text
let name: any = 'sueRimn';
name = 22;
```

**任意值的属性和方法**

在任意值上访问任何属性都是允许的：

```text
let anyThing: any = 'sueRimn';
console.log(antThing.name)
```

调用任何方法也是允许的：

```text
let anyThing: any = 'sueRimn';
anyThing.setName('八至');
anyThing.setAge(22);
```

**声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值**

**未声类型的变量**

变量如果在声明的时候未指定类型，那么会被识别为任意值类型：

```text
let something;
something = 'sueRimn';
something = 7;
```

**（7）联合类型**

联合类型（`Union Types`）表示取值可以为多种类型中的一种。

联合类型使用 `|` 分隔每个类型。

```text
let person: string | number;
person = 'sueRimn';
person = 22;
```

**a）访问联合类型的属性和方法**

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**：

```text
function getLength(something: string | number)：number {
    return something.toString()
}
// 以上，string和number的共有属性有toString，所以不会报错
```

### 四、泛型

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

### 五、自定义类型

### 类型别名

类型别名用来给一个类型起个新名字，使用 `type` 创建类型别名。

类型别名常用于联合类型。

```text
type Name = string;
type NameOne = () => string;
type NameTwo = Name | NameONe
function getName(n: nameTwo): Name {
    if (typeof n ==='string') {
        return n;
    } else {
        return n();
    }
}
```

### 六、类型推断

如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

TypeScript 会在没有明确的指定类型的时候推测出一个类型，这就是**类型推论**。

**如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成** `any` **类型，而完全不被类型检查**：

### 七、类型断言

类型断言（Type Assertion）：手动指定一个值的类型。

两种写法：

* 使用尖括号：`<类型>值`
* 使用as：值 as 类型

**在 tsx 语法（React 的 jsx 语法的 ts 版）中必须用后一种**

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**，当我们不确定类型的时候访问其中一个类型的属性或方式时，可以使用类型断言，比如：

```text
function getMsg(personal: string | number): number {
    if ((<string>personal).length) {
        return (<string>personal).length
    } else {
        return personal.toString().length
    }
}
```

类型断言的用法如上，在需要断言的变量前加上 `<类型>` 即可。

**类型断言不是类型转换，断言成一个联合类型中不存在的类型是不允许的**：

```text
function toBoolean(something: string | number): boolean {
    return <boolean>something;
}
​
// index.ts(2,10): error TS2352: Type 'string | number' cannot be converted to type 'boolean'.
//   Type 'number' is not comparable to type 'boolean'.
```

### 八、接口

对象的类型是接口，使用接口（`Interfaces`）来定义对象的类型。

接口一般首字母大写。

```text
interface Person {
    name: string,
    age: number
}
​
let my: Person = {
    name: 'sueRimn',
    age: 22
}
```

**赋值的时候，变量的形状必须和接口的形状保持一致**，即定义的变量数量必须和接口的属性数量一致，多或少都报错：

```text
// 报错
interface Person {
    name: string,
    age: number
}
​
let my: Person = {
    name: 'sueRimn',
}
​
// 报错
interface Person {
    name: string,
    age: number
}
​
let my: Person = {
    name: 'sueRimn',
    age: 22，
    gender: '女'
}
```

**（1）可选属性**

如果希望不完全匹配一个形状，就使用可选属性：

```text
interface Person {
    name: string,
    age?: number
}
    
let my: Person {
    name: 'sueRimn'
}
​
// 或者
let my: Person {
    name: 'sueRimn',
    age: 22
}
```

**注意：不允许添加未定义的属性**

**（2）任意属性**

当希望一个接口有任意的属性时：

```text
interface Person {
    name: string,
    age?: number,
    [propName: string]: any
}
​
let my: Person = {
    name: 'sueRimn',
    gender: 'female'
}
```

以上使用 `[propName: string]` 定义了任意属性取 `string` 类型的值。

**注意：一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**

```text
interface Person {
    name: string,
    age?: number,
    [propName: string]: any
}
​
let my: Person = {
    name: 'sueRimn',
    age: 25,
    gender: 'female'
}
// 报错
```

上例中，任意属性的值允许是 `string`，但是可选属性 `age` 的值却是 `number`，`number` 不是 `string` 的子属性，所以报错了。

**（3）只读属性**

希望字段只能再创建的时候被赋值， 那么可以使用只读属性`readonly`：

```text
interface Person {
    readonly id: number,
    name: string,
    age?: number,
    [propName: string]: any
}
​
let my: Person = {
    id: 1997
    name: 'sueRimn',
    gender: 'female'
}
​
// 报错
my.id = 1998; // 再次赋值报错
```

**注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**：

```text
interface Person {
    readonly id: number,
    name: string,
    age?: number,
    [propName: string]: any
}
// 报错
let my: Person = { // 没给id赋值
    name: 'sueRimn',
    gender: 'female'
}
​
my.id = 1998; // 再次赋值报错
```

### 九、类

