## 数组
    数组对象的作用是:使用单独的变量名来存储一系列的值
 
### 数组常用的方法及属性：
- arr.length;
- 获取值 var a = arr[0];
- 给数组赋新值 arr[1] = '新值';
- 将新元素添加到数组末尾，并返回新数组的长度-->arr.push(a,b,c,d,e...);
- 将新元素添加到数组开始，并返回新数组的长度-->arr.unshift(a,b,c,d,e...);
- 删除数组的最后一项，并返回被删除的元素-->arr.pop();
- 删除数字第一个元素，并返回被删除的元素-->arr.shift();
- arr.splice(1,2)-->从1的位置开始（包含1）向后删除两个元素，数组形式返回所移除的元素

```
var arr = [1,2,3,4,5,6,7];
var arr2 = arr.splice(1,2);
console.log('arr:%o, arr2: %o', arr, arr2) // arr: [1,4,5,6,7]   arr2: [2,3]
```
- arr.splice(1,2,a,b)可以在删除的位置添加元素
- slice(start,end)-->以数组的形式返回数组的一部分，不包括end位置的元素，如果省略end将复制start及之后的所有元素；
- join('分隔符')用数组的元素组成字符串，arr.join('%') --> 1%2%3%4...
- concat()方法用于合并数组并返回一个新数组；
- arr.reverse()将数组翻转（倒过来显示）
- sort()文字数组从字面上对数组进行排序;
```
sort(function (a,b) { return a - b;})--> 数字数组按值排序
```
- for in →用来循环输出数组中的元素
### 求两个数组的差集
```
let a = [1, 2, 3];
let b = [2, 3];
let c = a.filter(key => !b.includes(key))
console.log(c) // [1]
```
### 数组去除重复项返回新数组
```
var arr = [1,2,3,3,3,2]
arr = [...new Set(arr)]
console.log(arr) // [1,2,3]
```
---
## 对象

### 将给对象中的key设置为变量
```
var e = 'han'
var dd = {[`name${e}`]: 222}
console.log(dd) // namehan: 222
```

-------------------------------------------------- 待续