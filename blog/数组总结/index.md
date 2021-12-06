超详细的js数组方法（总结）

在平时的开发中我们经常会用的数组的内置方法，也是面试最常问到的东西。数组是js中最常用到的数据集合，其内置的方法有很多，熟练掌握这些方法，可以有效的提高我们的工作效率，同时对我们的代码质量也是有很大影响。

### 创建数组
1. 字面量表示
```
let arr = [] // 创建一个空数组
let arr1 = [1, 2] // 创建一个包含两项数据的数组
```
2. 使用Array构造函数
 + 无参数构造
 ```
 let arr = new Array() // 创建一个空数组
 ```
 + 带参数构造
 如果只传一个数值参数，则表示创建一个初始长度为指定数值的空数组
 ```
 let arr = new Array(5) // 创建一个数组长度为5的空数组
 ```
 如果传入一个非数值的参数或者参数个数大于 1，则表示创建一个包含指定元素的数组
 ```
 let arr = new Array(1,2,3) // 创建一个包含3个数值的数组
 ```
### 数组方法大全
数组原型方法主要有以下这些：
+ join()：返回值为用指定的分隔符将数组每一项拼接的字符串
+ push()：向数组的末尾添加元素，返回值是当前数组的length(修改原数组)
+ pop()：删除数组末尾的元素,返回值是被删除的元素（修改原数组）
+ shift()：删除首位元素，返回值是被删除的元素（修改原数组）
+ unshift()：向首位添加元素，返回值是数组的长度（修改原数组）
+ slice()：返回数组中指定起始下标和结束下标之间的（不包含结束位置）元素组成的新数组
+ splice()：对数组进行增删改（修改原数组）
+ fill()：使用特定值填充数组中的一个或多个元素(修改原数组)
+ filter()：过滤,数组中的每一项运行给定函数，返回满足过滤条件组成的数组
+ concat()：用于连接两个或多个数组
+ indexOf()：返回当前值在数组中第一次出现位置的索引
+ lastIndexOf()：返回查找的字符串最后出现的位置，如果没有找到匹配字符串则返回 -1。
+ every()：判断数组中每一项是否都符合条件
+ some()：判断数组中是否存在满足的项
+ includes()：判断一个数组是否包含指定的值
+ sort()：对数字的元素进行排序(修改原数组)
+ reverse()：对数组进行倒叙(修改原数组)
+ forEach()：循环遍历数组每一项(没有返回值)
+ map()：循环遍历数组的每一项（有返回值）
+ copyWithin(): 从数组的指定位置拷贝元素到数组的另一个指定位置中（修改原数组）
+ find(): 返回第一个匹配的值，并停止查找
+ findIndex(): 返回第一个匹配值的索引，并停止查找
+ toLocaleString()、toString():将数组转换为字符串
+ flat()、flatMap()：扁平化数组
+ entries() 、keys() 、values():遍历数组

### 方法示例
1. join()   
将所有数组元素结合为一个字符串,它的行为类似 toString()，但是您还可以规定分隔符,原数组不变
```
let arr = [1, 2, 3, 4, 5]
let arr1 = arr.join()
console.log(arr1)  // 1,2,3,4,5
console.log(arr.join('-')) // 1-2-3-4-5
```
2. push()和 pop() 
push() 向数组末尾添加元素，可以添加一个或多个元素
pop() 删除数组的最后一个元素并返回删除的元素
```
let arr = [1, 2, 3, 4, 5]
let arr1 = arr.push(6, 7)  
console.log(arr1) // 7
console.log(arr) // [1, 2, 3, 4, 5, 6, 7]
```
```
let arr = [1, 2, 3, 4, 5]
let arr2 = arr.pop()
console.log(arr2);   // 5
console.log(arr);   // [1, 2, 3, 4]
```
3. shift和unshift
shift删除首位元素，返回值是被删除的元素
unshift 向首位添加元素，返回值是数组的长度
```
let arr = [1, 2, 3, 4, 5]
let arr2 = arr.shift()
console.log(arr2)  // 1
console.log(arr)  // [2,3,4,5]
```
```
let arr = [1, 2, 3, 4, 5]
let arr2 = arr.unshift(0)
console.log(arr2)  // 6
console.log(arr)  // [0,1,2,3,4,5]
```
4. slice(起始下标，结束下标)返回数组中指定起始下标和结束下标之间的（不包含结束位置）元素组成的新数组，只有一个参数就返回从该参数下标到当前数组末尾的所有元素,当出现负数时，将负数加上数组长度的值（6）来替换该位置的数
```
let arr = [1, 2, 3, 4, 5]

let arr1 = arr.slice(1)
console.log(arr1)  //[2, 3, 4, 5]
let arr2 = arr.slice(1,3)
console.log(arr2) // [2, 3]
let arr3 = arr.slice(1, -2)
console.log(arr3) // [2, 3]
console.log(arr) // [1, 2, 3, 4, 5]
```
5. splice() 
+ splice(index, howmany, item1, ....., itemX) 
index	必需。指定在什么位置添加/删除项目，使用负值指定从数组末尾开始的位置。
howmany	可选。要删除的项目数。如果设置为 0，则不会删除任何项目。
item1, ..., itemX	可选。要添加到数组中的新项目。
```
let arr = [1, 2, 3, 4, 5]
let arr1 = arr.splice(0, 2, 6, 7);
console.log(arr) // [6, 7, 3, 4, 5]
console.log(arr1) // [1, 2]
```
6. fill(value, start, end)es6新增
使用制定的元素填充数组，其实就是用默认内容初始化数组
value：填充值。
start：填充起始位置，可以省略。
end：填充结束位置，可以省略，实际结束位置是end-1。
```
let arr = [1, 2, 3, 4, 5]
let arr1 = arr.fill(6);
console.log(arr1) // [6, 6, 6, 6, 6]
console.log(arr) // [6, 6, 6, 6, 6]

let arr2 = arr.fill(6, 1, 2);
console.log(arr2) // [1, 6, 3, 4, 5]
```
7. filter()
```
let arr = [1, 2, 3, 4, 5]
let arr2 = arr.filter(function(item, index) {
 return index % 3 === 0 || item >= 4;
})
console.log(arr2);  //[1, 4, 5]
```
8. concat（）
用于连接两个或多个数组，不会改变原有数组返回一个数组的副本
```
let arr = [1, 2, 3, 4, 5]
let arr1 = ['a', 'b', 'c', 'd']

let arr3= arr.concat(6,arr1)
console.log(arr)  // [1, 2, 3, 4, 5]
console.log(arr1)  // ['a', 'b', 'c', 'd']
console.log(arr3)  // [1, 2, 3, 4, 5, 6, 'a', 'b', 'c', 'd']
```
可以看到第一个参数不是数组，但concat方法还是把它拼接到了arr数组的后面
9. indexOf(item,start)和lastIndexOf(item,start)
item	查找的元素
start	可选的整数参数。开始检索的位置。如省略该参数，则将从字符串的首字符开始检索。
item	查找的元素
start	可选的整数参数。开始检索的位置。如省略该参数，则将从字符串的最后一个字符处开始检索。
```
let arr = [1, 2, 3, 4, 5, 6, 3]
console.log(arr.indexOf(3)) // 2
console.log(arr.lastIndexOf(3)) // 6
console.log(arr.indexOf(3, 2)) // 2
console.log(arr.lastIndexOf(3,4)) // 2
console.log(arr.indexOf("3")) // -1
```
10. every()判断数组中每一项都是否满足条件，只有所有项都满足条件，才会返回 true。
```
let arr = [1, 2, 3, 4, 5, 6, 10, 11]
let arr2 = arr.every(function(x) {
 return x < 10;
})
console.log(arr2);  // false
let arr3 = arr.every(function(x) {
 return x < 20;
})
console.log(arr3);  // true
```
11. some()判断数组中是否存在满足条件的项，只要有一项满足条件，就会返回 true
```
let arr = [1, 2, 3, 4, 5, 6, 10, 11]
let arr2 = arr.some(function(x) {
 return x < 10
});
console.log(arr2);  // true
let arr3 = arr.some(function(x) {
 return x > 20
});
console.log(arr3);  // false
```
12. includes(searchElement，fromIndex) es7 新增,检测数组中是否包含一个指定的值，如果是返回true，否则false。
searchElement	需要查找的元素
fromIndex	可选。从该索引处开始查找，默认为 0。
```
let arr = [1, 2, 3, 4, 5, 6, 10, 11]
let arr1 = arr.includes(31);
console.log(arr1); // false
let arr2 = arr.includes(6, 3)
console.log(arr2); // true
```
13. sort()
对数组元素排序，没有参数时按照字母顺序（字符编码的顺序）进行排序
```
let arr1 = ["a", "d", "c", "b"];
console.log(arr1.sort());   // ["a", "b", "c", "d"]
let arr2 = [13, 24, 51, 3];
console.log(arr2.sort());   // [13, 24, 3, 51]
console.log(arr2);   // [13, 24, 3, 51](原数组被改变)
```
array.sort(compareFunction)可以通过提供“比较函数”来排序，该函数应返回负值、零值或正值，具体取决于参数，例如：
function(a, b){return a-b}
如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等则返回 0，如果第一个参数应该位于第二个之后则返回一个正数。
14. reverse（）
用于颠倒数组中元素的顺序
```
let arr = [13, 24, 51, 3];
console.log(arr.reverse());   //[3, 51, 24, 13]
console.log(arr);   //[3, 51, 24, 13](原数组改变)
```
15. forEach()对数组进行遍历循环，对数组中的每一项运行给定函数。没有返回值   
array.forEach(function(currentValue, index, arr),thisValue)
function(currentValue, index, arr)	必需。为数组中的每个元素运行的函数。
+ currentValue	必需。当前值。
+ index	可选。索引。
+ arr	可选。当前元素所属的数组对象
thisValue	可选。要传递给函数以用作其 "this" 值的值。如果此参数为空，则值 "undefined" 将作为其 "this" 值传递。
```
let arr = [1, 2, 3, 4]
arr.forEach((item,index,ar) => {
  console.log(item + '|' + index + '|' + (ar === arr))
})
1|0|true
2|1|true
3|2|true
4|3|true
```
16. map()对数组中的每一项运行给定函数并返回处理后的值,不会改变原数组
```
let arr = [1, 2, 3, 4, 5, 6, 10, 11]
let arr2 = arr.map(function(item){
 return item + 1
});
console.log(arr2) // [2, 3, 4, 5, 6, 7, 11, 12]
console.log(arr) // [1, 2, 3, 4, 5, 6, 10, 11]
```

17. copyWithin(target, start, end) [es6 新增]从数组的指定位置拷贝元素到数组的另一个指定位置中
target	必需。复制到指定目标索引位置。   
start	可选。元素复制的起始位置。
end	可选。停止复制的索引位置 (默认为 array.length)。如果为负值，表示倒数。
```
let arr = [1, 2, 3, 4, 5, 6, 10, 11]
arr.copyWithin(1, 2, 3);
console.log(arr) // [1, 3, 3, 4, 5, 6, 10, 11]
```
18. find((currentValue, index, arr) => {}, thisValue)和 findIndex((currentValue, index, arr) => {}, thisValue)
(currentValue, index, arr) => {}	必需。为数组中的每个元素运行的函数。
+ currentValue	必需。当前元素值。
+ index	可选。当前元素的索引。
+ arr	可选。当前元素所属的数组对象
thisValue	可选。要传递给函数以用作其 "this" 值的值。如果此参数为空，则值 "undefined" 将作为其 "this" 值传递。
二者的区别是：find()方法返回匹配的值，而 findIndex()返回匹配位置的索引。
```
let arr = [1, 2, 3, 4, 5, 6, 10, 11]
let a = arr.find((value, keys, arr) => {
    return value > 2;
}) 
console.log(a) // 3 返回匹配的值
let b = arr.findIndex((value, keys, arr) => {
    return value > 2;
}) 
console.log(b) // 2 返回匹配位置的索引
```
find()和findIndex()方法都会在回调函数第一次查找结果为true时停止查找并返回第一个匹配的值
19. toLocaleString()和toString()讲数组转换为字符串
```
let arr = [1, 2, 3, 4, 5, 6, 10, 11]
const str = arr.toLocaleString();
const str1 = arr.toString()
console.log(str); // 1,2,3,4,5,6,10,11
console.log(str1); // 1,2,3,4,5,6,10,11
```
20. flat()、flatMap() [es6 新增]
flat()返回一个扁平化的数组
参数： 指定要提取嵌套数组的结构深度，使用Infinity，可展开任意深度的嵌套数组，默认值为 1
```
let arr = [0, 1, 2, [3, 4]]
let arr1 = arr.flat()
console.log(arr) // [0, 1, 2, [3, 4]]
console.log(arr1) // [0, 1, 2, 3, 4]

let arr2 = [0, 1, 2, [[[3, 4]]]]
let arr3 = arr2.flat(2)
console.log(arr3) // [0, 1, 2, [3, 4]]

let arr4 = arr2.flat(Infinity)
console.log(arr4)// [0, 1, 2, 3, 4]

// 如果原数组有空项，flat()方法会跳过空项
let arr5 = [1, 2, , 4, 5];
let arr6 = arr5.flat();
console.log(arr6) // [1, 2, 4, 5]
```
flatMap()对数组的每个成员执行函数，相当于执行Array.prototype.map(),然后对返回值组成的数组执行flat()方法。
```
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]
```
21. entries() 、keys() 、values()[es6 新增]
entries()，keys()和values() —— 用于遍历数组。它们都返回一个遍历器对象，可以用for...of循环进行遍历
区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历
```
let arr = ['a', 'b', 'c', 'd']
for (let index of arr.keys()) {  
  console.log(index)
}  
// 0  
// 1  
// 2
// 3
for (let item of arr.values()) {  
  console.log(item)
}  
// a   
// b   
// c
// d
for (let [index, item] of arr.entries()) {  
  console.log(index, item)
}  
// 0 "a"  
// 1 "b"
// 3 "c"  
// 4 "d"
```
或者手动调用遍历器对象的next方法，进行遍历
```
let arr = ['a', 'b', 'c', 'd']
let item = arr.entries()
console.log(item.next().value) // [0,  a ]  
console.log(item.next().value) // [1,  b ]  
console.log(item.next().value) // [2,  c ]
console.log(item.next().value) // [3,  d ]
console.log(item.next().value) // undefined
```