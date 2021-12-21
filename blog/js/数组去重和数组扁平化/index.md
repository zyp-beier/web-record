## 数组去重和数组扁平化
数组去重我们不但会在平时的开发中用到，就是面试也是高频出现的一道题,面试官的目的主要是想了解我们对基础知识的掌握以及是否能用更优雅的方法去实现。

### 数组去重
以下几种方法均采用   
`const arr = [1, 1, 2, 3, 3, 4, 5, 5, 6, 7, 7, 8, 9, 9, 10]`
1. 双循环去重 主要思想就是定义一个新数组，存放原数组的第一个元素，然后将原数组--和新数组的元素对比，若不同则存放在新数组里
```
function unique(arr) {
  let arr1 = [arr[0]]
    for(let i = 0; i < arr.length; i++ ) {
      let flag = true
      for(let j = 0; j < arr1.length; j++) {
        if (arr[i] === arr1[j]) {
          flag = false;
          break
        }
      }
      if (flag) {
        arr1.push(arr[i])
      }
    }
    return arr1
}
```
2. indexOf去重1 主要思想：创建一个新数组并判断元素是否已经在新数组出现过，没有则添加到新数组
```
function unique(arr) {
  let arr1 = []
  for (let i = 0; i < arr.length; i++) {
    if (arr1.indexOf(arr[i]) === -1 ) {
      arr1.push(arr[i])
    }
  }
    return arr1
}
```
3. indexOf 去重2 通过过滤返回元素第一次出现的位置与原下标相等所组成的新数组
```
function unique(arr) {
  let arr1 = []
  return arr.filter((item, index) => {
      return arr.indexOf(item) === index
  })
}
```
4. 相邻元素去重 将原数组排序然后相邻元素进行比对，如果不相同就添加到新数组
```
function unique(arr) {
  arr.sort()
  let arr1 = []
  for (let i = 0; i < arr.length; i++) {
      if (arr[i] !== arr[i-1]) {
          arr1.push(arr[i])
      }
  }
  return res
}
```
5. 利用对象属性存在的特性，如果没有该属性则存入新数组
```
function unique(arr) {
  let obj = {}
  let arr1 = []
  for(let i = 0; i < arr.length; i++) {
    if (!obj[arr[i]]) {
      obj[arr[i]] = true
      arr1.push(arr[i])
    }
  }
  return arr1
}
```
6. 利用数组原型对象上的includes方法。创建一个新数组并判断是否包含原数组的元素，不包含则添加到新数组
```
function unique(arr) {
  let arr1 = []
  for(let i = 0; i < arr.length; i++) {
    if (!arr1.includes(arr[i])) {
      arr1.push(arr[i])
    }
  }
  return arr1
}
```
7. 利用数组原型对象上的 splice 方法。
```
function unique(arr) {
  let length = arr.length;
  for (let i = 0; i < length; i++) {
      for (j = i + 1; j < length; j++) {
          if (arr[i] == arr[j]) {
              arr.splice(j, 1);
              length--;
              j--;
          }
      }
  }
  return arr;
}
```
8. 利用数组原型对象上的 lastIndexOf 方法。与上面第二种方法思想一致
```
function unique(arr) {
  let arr1 = []
  for(let i = 0; i< arr.length; i++) {
    if (arr1.lastIndexOf(arr[i]) === -1) {
      arr1.push(arr[i])
    }
  }
  return arr1
}
```
9. 利用 ES6的set方法。
```
function unique(arr) {
  //Set数据结构，它类似于数组，其成员的值都是唯一的
  return Array.from(new Set(arr)); // 利用Array.from将Set结构转换成数组
  // 或者
  // return [...new Set(arr)]
}
```
### 数组扁平化
将二维甚至多维数组变成一维数组
`const arr = [1,2,[3,4,[5,6],7],8]`
1. flat(Infinity)
```
let arr1 = arr.flat(Infinity)
```
2. for循环递归
```
const arr2 = []
function arr1(arr) {
  for(let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) {
      arr1(arr[i])
    }else{
      arr2.push(arr[i])
    }
  }
}
arr1(arr)
```
3. reduce
```
function arrFlat(arr) {
  return arr.reduce((pre,cur) => {
    return pre.concat(Array.isArray(cur) ? arrFlat(cur) : cur)
  }, [])
}
console.log(arrFlat(arr))
```