# 字符串操作方法

> [查看原文](https://frugencefidel.com/blogs/10-javascript-string-methods-you-should-know)

### 1、startsWith\(\)

检查字符串是否以指定的字符开头

```text
const STR = 'JavaScript is amazing';
console.log(STR.startsWith('JavaScript')); // true
console.log(STR.startsWith('Java')); // true
console.log(STR.startsWith('javascript')); // false

// 可选择位置
console.log(STR.startsWith('Script', 4)); // true
console.log(STR.startsWith('SCRIPT', 4)); // false
```

### 2、endsWith\(\)

检查字符串是否以指定的字符结束

```text
  const str = 'JavaScript is amazing';

  console.log(str.endsWith('amazing')); // true
  console.log(str.endsWith('ing')); // true
  console.log(str.endsWith('Amazing')); // false

  // 可选择长度
  // 如果不是特殊情况，一般长度指字符串的长度
  console.log(str.endsWith('is', 13)); // true
  console.log(str.endsWith('i', 13)); // false
  console.log(str.endsWith('s', 13)); // true
```

### 3、includes\(\)

检查字符串是否包含指定的字符

```text
  const str = 'JavaScript is amazing';

  console.log(str.includes('Script')); // true
  console.log(str.includes('script')); // false
  console.log(str.includes('array')); // false
```

### 4、slice\(\)

复制字符串的某些部分而不修改原字符串

```text
 const str = 'JavaScript is amazing';

  // 默认从索引0开始
  console.log(str.slice()); // 'JavaScript is amazing'

  // 从索引4开始复制
  console.log(str.slice(4)); // 'Script is amazing'

  // 在索引10处结束复制
  console.log(str.slice(0, 10)); // 'JavaScript'
```

### 5、toUpperCase\(\)

将字符串转换为大写字母

```text
const str = 'JavaScript is amazing';

console.log(str.toUpperCase()); // 'JAVASCRIPT IS AMAZING'
```

### 6、toLowerCase\(\)

将字符串转换为小写字母

```text
const str = 'JavaScript is amazing';

console.log(str.toLowerCase()); // 'javascript is amazing'
```

### 7、charAt\(\)

返回指定位置的字符

```text
const str = 'JavaScript is amazing';

  console.log(str.charAt()); // 'J'
  console.log(str.charAt(11)); // 'i'
  console.log(str.charAt(14)); // 'a'
  console.log(str.charAt(110)); // ''
```

### 8、split\(\)

将字符串拆分为子字符串数组

```text
  const str = 'JavaScript is amazing';
  const strNew = 'JavaScript-is-amazing';

  console.log(str.split()); // ["JavaScript is amazing"]

  // 分隔符字符串，用于确定在何处进行拆分
  console.log(str.split('S')); // ["Java", "cript is amazing"]
  console.log(str.split('is')); // ["JavaScript ", " amazing"]
  console.log(str.split(' ')); // ["JavaScript", "is", "amazing"]
  console.log(strNew.split('-')); // ["JavaScript", "is", "amazing"]
```

### 9、replace\(\)

用字符串中的另一个值替换指定的值，区分大小写

```text
  const str = 'JavaScript is amazing';

  console.log(str.replace('JavaScript', 'Node.js')); // 'Node.js is amazing'

  // replace() 方法区分大小写
  console.log(str.replace('Javascript', 'Node.js')); // 'JavaScript is amazing'

  // 使用正则表达式区分大小写
  console.log(str.replace(/Javascript/i, 'Node.js')); // 'Node.js is amazing'

  // 替换第一项
  console.log(str.replace('a', 'A')); // 'JAvaScript is amazing'

  // 替换符合条件的所有项
  console.log(str.replace(/a/g, 'A')); // 'JAvAScript is AmAzing'
```

### 10、repeat\(\)

返回现有字符串副本倍数的新字符串

```text
  const str = 'JavaScript';

  console.log(str.repeat(3)); // 'JavaScriptJavaScriptJavaScript'
  console.log(str.repeat(1)); // 'JavaScript'
  console.log(str.repeat(0)); // ''
```

