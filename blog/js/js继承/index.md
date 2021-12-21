对象的创建以及对象的继承
### 创建对象的六种方式
1. Object构造函数创建
```
let person = new Object()
person.name= 'Lisa'
person.age = 25
person.job = 'Software Engineer'
person.sayName = function() {
  console.log(this.name)
}
```
这里创建了一个名为person的对象，添加了三个属性和一个方法
2. 对象字面量
```
let person = {
  name: 'Lisa',
  age: 25,
  job: 'Software Engineer',
  sayName: function() {
    console.log(this.name)
  }
}
```
对象字面量是对象定义的一种简写形式，第一种和第二种方式其实都是一样的，只是写法上的区别不同
把创建对象的过程封装在函数体内，通过函数的调用直接生成对象。
3. 工厂模式创建对象
```
function createObj(name, age, job) {
  let p = new Object()
  p.name = name
  p.age = age
  p.job = job
  p.sayName = function() {
    return this.name
  }
  return p
}
console.log(createObj('Lisa', 25, 'Software Engineer'))
console.log(createObj('Lisa1', 26, 'Software Engineer1'))
```
4. 使用构造函数创建对象
```
 function Person(name, age, job) {
      this.name = name
      this.age = age
      this.job = job
      this.sayName = function() {
        return this.name
      }
    }
let person = new Person('Lisa', 25, 'Software Engineer')
let person2 = new Person('Lisa2', 26, 'Software Engineer2')
```
对比工厂模式有以下区别

没有显示的创建对象
直接将属性和方法赋值给this对象
不必将对象返回出来


5. 原型创建对象模式
```
function Person() {
     Person.prototype.name = 'Lisa'
     Person.prototype.age = 25
     Person.prototype.job = 'Software Engineer'
     Person.prototype.sayHello = function() {
       return this.name
     }
   }
let person1 = new Person()
let person2 = new Person()
person1.name = 'siri'
console.log(person1.name) // siri 来自实例
console.log(person2.name) // Lisa 来自原型
```
6. 混合模式（构造函数+原型模式）
```
   function Person( name, age, job) {
     this.name = name
     this.age = age
     this.job = job
   }
   Person.prototype.name = 'Lisa'
   Person.prototype.sayHi = function() {
     console.log('hi')
   }
   Person.prototype = {
    constructor: Person,
    say() {
      console.log(this.name)
    }
   }
   
let person1 = new Person('Lisa', 25, 'Software Engineer2')
let person2 = new Person('lida', 26, 'Software Engineer')
```
### 对象继承的几种方式
1.原型链继承
```
function SuperType(){
  this.property = true
}
SuperType.prototype.getSuperValue = function(){
  return this.property
}

function SubType(){
  this.subproperty = false
}
// 继承自SuperType
SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function (){
  return this.subproperty
}

var example = new SubType()
alert(example.getSuperValue()) //true
```
使用原型创建对象会存在多个实例对引用类型的操作被篡改的问题,如下
```
function SuperType(){
  this.colors = ['red', 'blue', 'green']
}
function SubType() {}
SubType.prototype = new SuperType()

let subType = new SubType()
subType.colors.push('black')
console.log(subType.colors) //['red', 'blue', 'green', 'black']
let subType2 = new SubType()
console.log(subType.colors) //['red', 'blue', 'green', 'black']
```
两个实例指向同一个原型对象，原型对象被改变，所有实例也跟着改变
2. 借用构造函数继承
使用call()方法和apply()方法将父类构造函数引入子类构造函数，使用父类的构造函数来增强子类实例，等同于复制父类的实例给子类
```
function SuperType(name) {
  this.name = name
  this.colors = ['red', 'green', 'pink']
}

function SubType(name, age) {
  this.age = age
  SuperType.call(this, name)
}
const subType = new SubType('Lisa', 25)
subType.colors.push('black')
console.log(subType.colors) // ['red', 'green', 'pink', 'black']
const subType2 = new SubType()
console.log(subType2.colors) // ['red', 'green', 'pink']
subType.age // 25
subType.name // Lisa
```
缺点：
+ 只能继承构造函数的属性和方法不能继承构造函数的原型属性和方法
+ 无法实现构造函数的复用， 每个子类都有父类实例函数的副本，影响性能，代码会臃肿
3. 组合继承
```

```

