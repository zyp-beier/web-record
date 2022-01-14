类的定义
类 可以理解为类型、类别，一个具有相同特征相同行为的对象的集合，也可以理解为一个模具，它可以生产某种特征相同的产品，并可以在产品的基础上再次改进，类包含一系列的对象，对象属于某一个类。

1.  类的定义
类有两种定义方式： 类声明和类表达式
两种方式都是使用class关键词 ➕ 大括号
+  类声明
```
 class Person {}
```
+ 类表达式
```
const Person = class {}
```
不管是类声明还是类表达式，没有定义之前都是不能使用的。并且建议类名首字母要使用大写
2. 类的构成
构造函数， 实例方法， 获取函数，设置函数以及静态类方法，但这都不是必须的
```
// 空类
class Person{}
// 带有构造函数
class Person{
  constructor(){}
}
// 获取函数
class Person{
  get name() {}
}
// 静态方法
class Person{
  static getName() {}
}
```
3. 类构造函数
使用new操作符创建类的新实例时会调用constructor构造函数
+ 实例化
需要使用new操作符来对构造函数进行实例化，new操作符主要做了以下事情
 1.在内存中创建一个新对象
 2.将构造函数的prototype属性赋值给新对象内部的__proto__（将构造函数的作用域赋值给新对象）
 3.执行构造函数（给新对象添加属性）
 4.返回这个对象
 ```
class Person {
  constructor(name) {
    this.name = name
  }
}
const person = new Person('liSa')  
console.log(person)  // Person {name: 'liSa'}
 ```
类构造函数必须使用new操作符，如果没有就会抛出错误。普通的构造函数如果不使用new操作符就会用全局的this作为内部对象。
4. 实例、原型和类成员
+ 实例成员
每次通过new实例化类的时候都会执行类构造函数，每个实例都对应一个唯一的成员对象，也就意味着实例与实例之间是不共享的
```
class Person{
  constructor() {
    this.name = new String('Lisa')
    this.color = ['red', 'green']
    this.sayName = () => {
      return this.name
    }
  }
}
const p1 = new Person
const p2 = new Person
console.log(p1.name, p2.name) // String {'Lisa'} String {'Lisa'}
console.log(p1.name === p2.name)  // false
console.log(p1.sayName() === p2.sayName()) // false
```
+ 原型方法
原型方法是可以在实例之间共享的
```
class Person{
  constructor(name) {
    this.name = name
    // 带有this的属性都将添加到各个实例中，因为this指向的是实例对象
    this.sayName = () => {
      return this.name
    }
  }
  shared() {
    return 'shared'
  }
}
const p1 = new Person(1)
const p2 = new Person(2)
console.log(p1.sayName() === p2.sayName()) // false
console.log(p1.shared() === p2.shared()) // true
```
+ 静态类方法
每个类中只能有一个静态类成员，静态类方法使用static关键字作为前缀，在静态类方法中this引用类自身。
```
class Person{
  constructor(name) {
    this.name = name
    // 带有this的属性都将添加到各个实例中，因为this指向的是实例对象
    this.say = () => {
      return 'p1'
    }
  }
  // 定义在类的原型对象中
  say() {
    return 'prototype'
  }
  // 定义在类本身中
  static say() {
    return 'Person'
  }
}
const p1 = new Person(1)
console.log(p1.say()) // p1
console.log(Person.prototype.say()) // prototype
console.log(Person.say()) // Person
```
5. 继承
原生支持类继承机制是es6新增的特性，虽然使用的是新语法底层依旧使用的是原型链

+ 1.基础继承
使用extends关键字，可以继承任何拥有Construct和原型的对象
```
class Person{}
class Man extends Person {}
const man = new Man()
console.log(man instanceof Man)  // true
console.log(man instanceof Person)  // true
```
派生类的方法可以通过super关键字引用它们的原型，这个关键字只能在类构造函数，实例方法以及静态方法中使用。
```
class Person{
  static say() {
    return 'Person'
  }
}
class Man extends Person {
  static say() {
    console.log(super.say())
  }
}
Man.say() // Person
```
如果在派生类中定义了构造函数，那么必须要调用super()或者在其中返回一个对象
```
class Person{}
class Man extends Person {
  constructor() {
    super()
  }
}
class Woman extends Person {
  constructor() {
    return {}
  }
}
const man = new Man()
const woman = new Woman()
```
6. 抽象基类
不允许被实例化且只能被继承的类，ECMAscript没有支持这种类的语法但可以通过new.target实现
```
// 抽象基类
class Person{
  constructor() {
    if (new.target === Person) {
      throw new Error('Person cannot be directly instantiated')
    }
  }
}
// 派生类
class Man extends Person {}
console.log(new Man())  // Man {}
console.log(new Person())  //Error: Person cannot be directly instantiated
```