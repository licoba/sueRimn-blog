---
description: 关于数组需要知道的十个方法
---

# 数组方法

> [查看原文](https://morioh.com/p/3ba421a8a63d)

### 1、forEach\(\)

循环遍历数组

```text
const ARR = [1, 2, 3, 4, 5];
ARR.forEach(item => {
    console.log(item);// 1, 2, 3, 4, 5
})
```

### 2、includes\(\)

检查数组是否包含方法中传递的项

```text
const ARR = [1, 2, 3, 4, 5];
ARR.includes(2); // true
ARR.includes(8); // false
```

### 3、filter\(\)

创建一个新数组，在新数组中过滤出符合条件的项

```text
const ARR = [1, 2, 3, 4, 5];
const NEWARR = ARR.filter(num => num > 3); // [4, 5]
console.log(NEWARR); // [4, 5]
console.log(ARR); // [1, 2, 3, 4, 5]
```

### 4、map\(\)

通过在每个元素中调用提供的函数来创建新数组。

```text
const ARR = [1, 2, 3, 4, 5];
const NEWARR = ARR.map(num => num + 1);// [2, 3, 4, 5, 6]
console.log(ARR); // [1, 2, 3, 4, 5]
```

### 5、reduce\(\)

对累加器和数组中的每个元素\(从左到右\)应用一个函数，将其缩减为一个值

```text
const ARR = [1, 2, 3, 4, 5];
const sum = arr.reduce((total, value) => total + value, 0);// 21
```

### 6、some\(\)

检查至少有一个数组项符合了该条件。如果符合，则返回“true”，否则返回“false”。

```text
const ARR = [1, 2, 3, 4, 5];
const NEWARR = ARR.some(num => num > 4); // true
const ANOTHERARR = ARR.some(num => num <= 0); // false
```

### 7、every\(\)

检查数组所有项是否都符合该条件。如果符合，则返回“true”，否则返回“false”。

```text
const ARR = [1, 2, 3, 4, 5];
const NEWARR = ARR.every(num => num > 4); // false
const ANOTHERARR = ARR.every(num => num < 8); // true
```

### 8、sort\(\)

用于按升序或降序排列/排序数组的项。

```text
const ARR = [1, 2, 3, 4, 5];
const alpha = ['e', 'a', 'c', 'u', 'y'];
 // 降序
const DESCORDER = ARR.sort((a, b) => a > b ? -1 : 1);
// [5, 4, 3, 2, 1]

// 升序
const ASCORDER = alpha.sort((a, b) => a > b ? 1 : -1);
// ['a', 'c', 'e', 'u', 'y']
```

### 9、Array.from\(\)

这将所有类数组或可迭代的东西都变成了数组，特别是在使用DOM时，这样就可以使用其他数组方法，比如reduce、map、filter等等。

```text
const name = 'sueRimn'; // frugence
const nameArray = Array.from(name); // ['s', 'u', 'e', 'R', 'i', 'm', 'n']
```

**使用DOM时**

```text
const LIS = document.querySelectorAll('li');
const LISARRAY = Array.from(document.querySelectorAll('li'));

console.log(Array.isArray(LIS)); //  false
console.log(Array.isArray(LISARRAY));  //  true
```

### 10、Array.of\(\)

用传入的每个参数创建数组

```text
const ARR = Array.of(1, 2, 3, 4, 5, 6); // [1, 2, 3, 4, 5, 6]
```

