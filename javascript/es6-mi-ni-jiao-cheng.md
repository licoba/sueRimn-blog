# ES6迷你教程

### 1、放弃使用`var`

不要再使用`var`，使用：

* `let`：用于变量，值可变
* `const`：用于常量，值不可变

```text
let num = 0;
num = 1;
console,log(num); // 1

const NEWNUM = 0;
NEWNUM = 1; // 抛出错误
```

### 2、使用箭头函数

* 箭头函数自动绑定`this`
* 一个参数时可以不写括号，其他情况必须带括号（无参或者多个参数）
* 不使用箭头函数的{}时，它将自动返回

```text
// 自动绑定this
Class Person {
    arrow () {
        return () => {
            console.log(this); // Person
        }
    }
}

// 一个参数时可以不写括号，其他情况必须带括号（无参或者多个参数）
const arrow = () => {}
const arrow = obj => {}
const arrow = (obj) => {}
const arrow = (obj, item) => {}

// 不使用箭头函数的{}时，它将自动返回
const arrow = num => num + 1;
const arrow = num => {
    return num + 1;
}
```

### 3、为参数定默认值

```text
const arrow = (arg1, arg2 = 1) => arg1 + arg2;
```

### 4、解构赋值

通过解构赋值，可以从对象或数组中提取特定的值。

在这里可以使用命名参数。

```text
// 从对象中获取特定的值
const obj = {
    one: '1',
    two: '2',
    three: '3'
}
const extractOfObject = myObj => {
    const { one } = myObj;
    console.log(one); 
}
extractOfObject(obj); // 1

// 从数组获取特定的值
const arr = ['s', 'u', 'e', 'R', 'i', 'm', 'n'];
const extractOfArray = myArr => {
    const [one, two] = myArr;
    console.log(onw, two);
}
extractOfArray(arr); // s u
```

### 5、模块导入/导出

从一个模块导出一个默认值以及使用析构赋值导入任意多个命名值。

**命名值一定要使用解构赋值导入**。

```text
// 导出 export ：

// 导出命名对象
export const obj = {
    "name": "sueRimn",
    "age": 22
}

// 导出命名数组
export const arr = ["sueRimn", 22]

// 导出唯一默认值
export default const only = {
    obj, // 等于 obj: obj
    arr  // 等于 arr: arr
}

// 导入 import ：
import { obj, arr } from './data' // 导入命名值
import only form './data'         // 导入默认值
import * as all from './data'     // 整体导入
```

### 6、使用`...`

`rest`和`spread`是不同的，概念相似。

用三个点（`...`）获取一个对象或数组的剩余值，叫`rest`。

用三个点（`...`）将一个对象或数组转换成一个新的对象或数组，叫`spread`，即扩展运算符。

* 使用`rest`获取剩余值

```text
// 数组使用扩展运算符
const arr = ['sueRimn', '八至', 22];
const restOfArr = myArr => {
    const [one,  ...rest] = myArr;
    console.log(one); // sueRimn
    console.log(rest); // 八至 22
}
restOfArr(arr);
// sueRimn
// 八至 22

// 对象使用扩展运算符
const obj = {
    'name': 'sueRimn',
    'age': 22,
    'gender': '女'
}
const restOfObj = myObj => {
    const { one, ...rest } = myObj;
    console.log(one); // sueRimn
    console.log(rest); // 22 女
}
restOfObj(obj);
// sueRimn
// 22 女
```

* 使用`spead`转换对象和数组为新对象和数字

```text
const my = {
    name: 'sueRimn',
    gender: '女'
}

const myInfo = {
    age: 22,
    ...my
}
console.log(myInfo); // {age: 22, name: 'sueRimn', gender: '女'}
```

### 7、使用模块字符串

反引号包装字符串，叫**模板字符串**。在反斜杠内部，使用`${}`来执行字符串插值。

```text
const str = `sueRimn is beautiful`;
const adj = 'cute';
const my = str +  `and ${adj}`;
console.log(my); // sueRimn is beautiful and cute!（脸皮厚）
```

### 8、类

`ES6` 的`class`可以看作只是一个语法糖，只是让对象原型的写法更加清晰、更加面向对象编程的语法，实现的功能和`ES5`是一样的。

* 类的默认方法`constructor`默认添加

```text
Class Person {
    constructor (x, y) { // 构造方法
        this.name = 'sueRimn'; // this代表实例对象
        this.gender = '女'
    }
    my () {
        return 'my name is ' + this.name + ',' + this.gender;
    }
}
```

* 类的实例：使用`new`命令生成类的实例

```text
new people = new Person('八至', '女')
```

* 类的继承：通过`extends`实现，这在`react`中是常见的组件写法

```text
class Girl extends from Person {
    constructor(name, gender, age) {
        super(name, gender, age); // super表示父类的构造函数，用来新建父类的this对象
        this.name = name;
        this.gender = gender;
        this.age = age;
    }
    
    my () {
        return super.my() + ',' + this.age;// 调用父类的my()方法
    }
}
```

