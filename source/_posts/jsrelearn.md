---
title: 面向对象
date: 2018-03-19 16:14:49
tags: [js, 进阶, 笔记]
categories: [技术笔记]
comments: false
---

## Object 对象

### 基本概念

JS 中其他**所有对象都继承自Object对象**，即那些对象都是Object的实例。
Object对象的原生方法分成两类：`本身`和`实例`方法。

### Object() 工具方法

Object 本身是一个函数,
可以把**任意参数**转换成对象,
如果参数是`对象则直接返回`该参数,
如果参数是`原始类型`的值, 则将其转为对应的`包装对象的实例`.

<!-- more -->

### Object 构造函数

Object构造函数的首要用途，是直接通过它来生成新对象。

```js
var obj = new Object(); // 通过构造函数生成新对象
var obj = {};           // 字面量写法等价与前者
```

构造函数与工具方法基本一样,不同的地方是,
Object(value) 是将值`转换`成对象,
new Object() 是`生成`一个对象.

### Object 的静态方法

> 所谓“静态方法”，是指部署在Object对象自身的方法。

| get相关操作                     | 含义                                |
| ------------------------------- | ----------------------------------- |
| Object.values(obj)              | 以数组形式返回对象的value(不含枚举) |
| Object.keys(obj)                | 以数组形式返回对象的key(不含枚举)   |
| Object.getOwnPropertyNames(obj) | 以数组形式返回对象的key(含枚举)     |

| 对象属性模型相关                  | 含义                         |
| --------------------------------- | ---------------------------- |
| Object.getOwnPropertyDescriptor() | 获取某个属性的描述对象。     |
| Object.defineProperty()           | 通过描述对象，定义某个属性。 |
| Object.defineProperties()         | 通过描述对象，定义多个属性。 |

| 控制对象状态               | 含义                     |
| -------------------------- | ------------------------ |
| Object.preventExtensions() | 防止对象扩展。           |
| Object.seal()              | 禁止对象配置。           |
| Object.freeze()            | 冻结一个对象。           |
| Object.isExtensible()      | 判断对象是否可扩展。     |
| Object.isSealed()          | 判断一个对象是否可配置。 |
| Object.isFrozen()          | 判断一个对象是否被冻结。 |

| 原型链相关              | 含义                                             |
| ----------------------- | ------------------------------------------------ |
| Object.create()         | 该方法可以指定原型对象和属性，返回一个新的对象。 |
| Object.getPrototypeOf() | 获取对象的Prototype对象。                        |

### Object 的实例方法

| 方法                                    | 含义                                                                       |
| --------------------------------------- | -------------------------------------------------------------------------- |
| Object.prototype.valueOf()              | 返回当前对象对应的值。                                                     |
| Object.prototype.toString()             | 返回当前对象对应的字符串形式。                                             |
| Object.prototype.toLocaleString()       | 返回当前对象对应的本地字符串形式。                                         |
| Object.prototype.hasOwnProperty()       | 判断某个属性是否为`当前对象自身`的属性，还是`继承自原型`对象的属性。       |
| in 运算符   `'length' in Date`          | 返回一个布尔值，表示一个对象是否具有某个属性, 不区分属性是自身还是来自继承 |
| Object.prototype.isPrototypeOf()        | 判断当前对象是否为另一个对象的原型。                                       |
| Object.prototype.propertyIsEnumerable() | 判断某个属性是否可枚举。                                                   |

- Object.prototype.valueOf() 方法的主要用于是用于`隐式转换`, 当与数值相加时会用 toString() 方法得到本身值在进行计算
- Object.prototype.toString() 方法作用是返回一个`对象形式`的字符串

### toString() 的应用：判断数据类型

> Object.prototype.toString 返回对象的`类型字符串`, 因此可以用来判断一个值的类型.

由于实例对象可能会自定义 `toString` 方法, 所以不会再去引用 Object.prototype 上的 `toString` 方法,
所以为了得到类型字符串, 最好直接使用 `Object.prototype.toString` 方法.
通过函数的 call 方法, 可以在任意值上调用这个方法, 帮助我们判断这个值得类型.

```js
Object.prototype.toString.call(2) // "[object Number]"
Object.prototype.toString.call([]) // "[object Array]"
......
```

## 面向对象编程

### 概念

为什么要了解面向对象编程呢?
因为要是不懂面向对象编程, 看框架的源码是很费劲的,
而面向对象也是不容易掌握的, 其中最重要的一定就是`抽象思维` 把思维要`打开`。

**对象是单个`实物`的抽象**
**对象是一个容器, 封装了属性(property) 和方法(method)**

### 构造函数

面向对象编程的第一步，`就是要生成对象`。
前面说过，对象是`单个实物的抽象`。
通常需要一个模板，表示`某一类实物`的`共同特征`，然后对象根据这个模板生成。

JS 使用构造函数(constructor)作为对象的模板.
所谓 "构造函数" ,就是专门用来生成实例对象的函数.
他就是对象的一个模板,用来描述对象的基本结构.
一个构造函数, 可以生成多个实例对象,这些实例对象都有相同的结构.

构造函数就是一个普通的函数, 但是有自己的特征和用法.

```js
var Vehicle = function () {
  this.price = 1000;
}
```

Vehicle 就是构造函数, 为了与普通函数的区别, 构造函数名字的第一个字母通常大写.
构造函数的特点有俩个.

- 函数体内使用了 `this` 关键字, 代表了所要生成的`对象实例`
- 声称对象时必须使用 `new` 命令

### new 命令

> new 命令的作用, 就是执行构造函数, 返回一个实例对象.

如果忘了使用 new 命令, 直接调用构造函数时,
构造函数就变成了普通函数, 并不会生成实例对象,
而且构造函数内的 `this` 会指向 `window`,
严格模式 this 默认指向 `undefined` 导致严格模式会出现错误

#### new 命令的原理

使用 new 命令是, 它后面的函数一次执行下面的步骤.

1. 创建一个`空对象`, 作为将要返回的对象实例.
2. 将这个空对象的原型, 指向构造函数的 `prototype` 属性
3. 将这个`空对象赋值给`函数内部的 `this` 关键字
4. 开始`执行`构造函数内部的代码

如果对`使用new`命令的函数内部`没有this`关键字的话, 返回的就是一个`空的对象`.
如果构造函数内部`有return` 语句并且`是对象`, `则直接返回`该对象, 反之则忽略, 继续返回 this 对象.

函数内部可以使用 `new.targe` 属性来区分是否使用 new 命令.

#### Object.create() 创建实例对象

> 构造函数作为模板, 可以生成实例对象.

但有时是拿不到构造函数, 只能拿到一个现有的对象.
这时就可以使用 Object.create() 方法就可以继承现有对象的方法和属性到新的对象上.

```js
// 原型对象
var A = {
  print: function () {
    console.log('hello');
  }
};

// 实例对象
var B = Object.create(A);

Object.getPrototypeOf(B) === A // true
B.print() // hello
B.print === A.print // true
```

## prototype 对象

> 面向对象编程很重要的一个方面, 就是对象的继承.
> A 对象通过继承 B 对象, 就能直接拥有 B 对象的所有属性和方法.
> 这对于代码的复用是非常有用的

### prototype 属性的作用

JavaScript 继承机制的设计思想就是,
原型对象的所有属性和方法, 都能被`实例对象共享`.
也就是说, 如果属性和方法定义在原型上,
那么所有实例对象就能共享, 不仅节省了内存, 还体现了是实例对象之间的联系

### 构造函数的缺点

构造函数中的方法每新建一个实例, 就会新建`构造函数`中的方法,
这样既没有必要, 又浪费系统资源, 因为所有的`构造函数`中所有的方法都是同样的行为,
**所以应该共享**, 可以通过 JS 的原型链实现该需求.

### 原型链

JavaScript 规定, **所有对象都有自己的原型对象**.
一方面, 任何一个对象, 都可以充当`其他对象的原型`,
另一方面, 由于原型对象也是对象, 所以它也`有自己的原型`.
因此, 就会形成一个 "原型链": 对象到原型, 再到原型的原型

```
如果一层一层的扒, 所有的原型追到底 都是 Object.prototype,
就是 Object 的 prototype 属性. 也就是说 所有对象都集成了 Object.prototype 的属性.
这也就是所有对象都有 valueOf 和 toString 方法的原因, 因为这是从 object.prototype 继承的.
```

那么这个顶级对象的原型有`指向何处`呢 ?
答案是 null, null 没有任何属性和方法, 也没有自己的原型.
_因此, 原型链的尽头就是 `null`._

```js
Object.getPrototypeOf(Object.prototype) //null
```

读取对象的某个属性时, JavaScript 引擎会先从自身对象寻找,
找不到就会去该属性对象的原型上去找, 以此类推到顶级对象,
如果还是找不到则返回 `undefined`.

注意, 一层一层的在整个圆形脸上寻找某个属性,
对性能是有影响的. 所寻找的属性在月上层的原型对象, 对性能的影响越大.
如果寻找某个不存在的属性时, 将会遍历整个原型链.

- **constructor 属性**
  - prototype 对象有一个 constructor 属性, 默认指向 prototype 对所在的构造函数.
- **instanceof 运算符**
  - 返回一个布尔值, 表示对象是否为某个构造函数的实例.

### this 关键字

#### 涵义

this 可以用在构造函数之中, 表示实例对象.
除此之外, this 还可以用在别的场合, this 都有一个共同点: 它总是返回一个对象

简单说, this 就是属性或方法 "当前" 所在的对象

```js
var a = {
  name: '张三',
  fn: function () {
    console.log(this)
  }
}
a.fn()
// { name:'张三', fn: f }
```

#### 适用场合

this 主要有以下几个使用场合

**1.全局环境**
全局环境使用 this , 它指的就是顶层对象 window.

**2.构造函数**
构造函数中的 this, 指的是实例对象.

**3.对象的方法**
如果对象的方法里面包含 this, this 的只想就是方式运行时所在的对象.
_该方法赋值给另一个对象时就会改变 this 指向._

**`this` 使用注意点:**

- 避免`多层` this
- 避免`数组处理`方法中的 this
- 避免`回调函数`中的 this

#### 绑定 this 的方法

虽然 this 的切换为 js 提供了超高的灵活性，但同时也加大了代码的可阅读性和困难程度。
有时就需要 将 this 固定下来， 避免出现意想不到的情况。
JavaScript 中提供的三种方法， 分别是 call、apply、bind。

- apply、call、bind 三者第一个参数都是this要指向的对象，也就是想指定的上下文；
- apply、call、bind 三者都可以利用后续参数传参；
- bind 是返回对应回调函数, 调用时改变其该bind函数的内部this

##### Function.prototype.call()

函数实例的 `call` 方法, 可以改变函数内部的 `this` 指向为 `call` 传入的第一个参数对象.

`call` 方法的`参数`需要是一个`对象`, 如果参数是 `null` 或 `undefined` 那么默认传入全局对象 `window`

```js
var n = 123;
var obj = { n: 456 };

function a() {
  console.log(this.n);
}

a.call() // 123
a.call(null) // 123
a.call(undefined) // 123
a.call(window) // 123
a.call(obj) // 456
```

`call` 方法还接受多个参数

```js
function add(a, b) {
  return a + b
}
add.call(this, 3, 5)
// 8
```

##### Function.prototype.apply()

apply 方法的作用于 call 基本一致, 唯一的区别就是它是用数组进行传参的.

```js
func.apply(thisValue, [arg1, arg2, ...])
```

##### Function.prototype.bind()

`bind` 方法用于将函数体内的 `this` 绑定到某个对象, 然后返回一个新函数.
