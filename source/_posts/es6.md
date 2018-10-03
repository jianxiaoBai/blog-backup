---
title: ES6 笔记
date: 2017-06-23 10:15:10
tags: [ES6, 笔记]
keywords: [ES6,笔记]
categories: [技术笔记]
comments: false
---


## 前言

 - 对阮一峰的ES6的内容进行的 `提炼`
 - 对其中的内容进行了 `简化`
 - 对一些我个人认为较复杂且不常用的功能或概念 `采取忽略`
 - 对ES6 `不熟悉` 的同学 可能会有一点帮助
 - 如笔记整理有错误还请留言 [指出](https://github.com/jianxiaoBai/jianxiaobai.github.io/issues)
 - 深度阅读请访问 [http://es6.ruanyifeng.com](http://es6.ruanyifeng.com)

<!-- more -->

------


## let 与 const

<p id="border-red">代码块：外层作用域无法读取内层作用域中的变量</p>

  - 共同点
      + 只在块级作用域中有效
      + 暂时性死区
      + 不允许重复声明
      + 不存在变量提升
  - 不同点
      + const 声明时 <span id="inline-red">必须赋值</span> 并且 <span id="inline-blue">值不可变</span>

---------


## 关于解析赋值


### 数组的

<p id="border-blue">数组的元素是按 <span id="inline-blue">次序排列</span> 的，变量的取值要与位置匹配</p>

**典型例子**

```javascript

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []

let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```



>**设置默认值** ：解构赋值允许指定默认值。

```javascript
let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'

-------------------------------------------
function f() {
  console.log('aaa');
}
// 函数 f 没有执行，因为 x 的值是 1 不属于 undefined
let [x = f()] = [1];

```
-------------

### 对象的

<p id="border-blue">对象的属性虽没有次序，但变量必须与属性同名</p>

**典型例子**

```javascript

let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
-------------------------------------------
/* 如果变量名与属性名不一致，必须写成下面这样。*/

/***********
注意:对于let和const来说，变量不能重新声明，所以一旦赋值的变量以前声明过，就会报错。
************/

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'

-------------------------------------------
/***********
下面代码中，let命令下面一行的圆括号是必须的，否则会报错。
因为解析器会将起首的大括号，理解成一个代码块，而不是赋值语句。
************/

let foo;
({foo} = {foo: 1}); // 成功

let baz;
({bar: baz} = {bar: 1}); // 成功

```


>**设置默认值** ：解构赋值允许指定默认值。


```javascript

var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x:y = 3} = {};
y // 3

var {x:y = 3} = {x: 5};
y // 5

var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"


```

------------

### 函数的

>经典范例

```javascript
/* 函数move的参数是一个对象，如果通过解构，得不到变量x或y的值，那么x和y等于默认值。*/
function move({x = 0, y = 0} = {}) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]. 因为 x 有值 就会赋值 ，y 没有值 就是默认值
move({}); // [0, 0]
move(); // [0, 0]
-------------------------------------------
/* undefined就会触发函数参数的默认值。 */
[1, undefined, 3].map((x = 'yes') => x);
// [ 1, 'yes', 3 ]
```

-----------

### 基本用途

####  交换变量的值

>上面代码交换变量x和y的值，这样的写法不仅简洁，而且易读，语义非常清晰。

```javascript
let x = 1;
let y = 2;

[x, y] = [y, x];
```

####  从函数返回多个值

>函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便。

```javascript
// 返回一个数组

function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```

#### 函数参数的定义

>解构赋值可以方便地将一组参数与变量名对应起来。

```javascript
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

#### 提取JSON数据

>解构赋值对提取JSON对象中的数据，尤其有用。

```javascript
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```

#### 函数参数的默认值

>指定参数的默认值，就避免了在函数体内部再写var foo = config.foo || 'default foo';这样的语句。

```javascript
jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
}) {
  // ... do stuff
};
```

#### 遍历Map结构

>ap结构原生支持Iterator接口，配合变量的解构赋值，获取键名和键值就非常方便。

```javascript
var map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
-------------------------------------------
/* 如果只想获取键名，或者只想获取键值，可以写成下面这样。 */

// 获取键名
for (let [key] of map) {
  // ...
}

// 获取键值
for (let [,value] of map) {
  // ...
}


```

#### 输入模块的指定方法

>加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。

```javascript
const { SourceMapConsumer, SourceNode } = require("source-map");
```

---------

## 关于类型的扩展

### 字符串扩展--模板字符串

```javascript

// 普通字符串输出：  '\n' 换行，trim（） 消除字符前后空格
` In JavaScript '\n' is a line-feed. `.trim()

// 字符串中嵌入变量 :  `${}`
var name = "Bob";
`Hello ${name},`

// 大括号内部可以进行运算
var x = 1，y = 2;

`${x + y}`
// "3"

// 大括号内部可以调用函数
function fn() {
  return "Hello World";
}

`foo ${fn()} bar`
// foo Hello World bar

```


---------

### 正则的扩展--后行断言

<p id="border-yellow">目前，有一个 <span id="inline-red">提案</span> ，引入后行断言。</p>

```javascript

/* 先行断言 ,只匹配百分号之前的数字 */
/\d+(?=%)/.exec('of US presidents 100%  have been male')  //["100"]

/* 后行断言 ,只匹配百分号之后的数字 */
/(?<=\$)\d+/.exec('Benjamin Franklin is on the $100 bill') //["100"]

```

------

### 数值的扩展

|方法|作用|
|----|----|
|Number.isNaN()|判断值是否为NaN，为NaN返回true|
|Number.parseInt()/parseFloat()|ES6将这俩个全局方法移植到了Number对象上|
|Number.isInteger()|判断值是否为整数|
|Math.trunc()|返回整数部分|
|Math.sign()|判断一个数是正数、负数、0，对应返回值 +1、-1、0|

---------

### 数组的扩展

#### Array.from(对象，\_对象处理函数，\_this指向)

可将 `数组的对象` 和 `可遍历的对象` 转换为数组结构

```javascript
/* 类似数组的对象 */
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']

/******************************************************/

/* 可遍历的对象 */
let namesSet = new Set(['a', 'b']) // nameSet {'a','b'}
Array.from(namesSet) // ['a', 'b']
```

#### Array.of()
将一组值转换为数组

```javascript
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3,2).length // 2
```

#### find()、findIndex()、includes()

 - find 方法，用于找出 `第一个` 符合条件的数组中的值，否则返回 undefined
 - findIndex 返回 `第一个` 符合条件的位置，否则返回-1。
 - includes() 和 indexOf() 很像，前者返回布尔值后者返回数值

```javascript
[1, 4, -5, 10].find((n) => n < 0)
// -5

[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```

#### 数组的遍历

entries() 、keys()、values()

```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

--------------

### 函数的扩展

#### 函数参数的默认值

 - 函数内部是不可以再次声明形参
 - 函数的执行时形参会形成作用域
   + 函数体内>形参内>全局内

```javascript
/* 如实例化时x、y没有传入参数，那么x、y的默认值就是 0 */
function Point(x = 0, y = 0) {
  this.x = x;
  this.y = y;
  //
}
var p = new Point();
p // { x: 0, y: 0 }

/* 调用时不传参数会报错 */
function foo({x, y = 5}) {
  console.log(x, y);
}

foo({}) // undefined, 5 , 因为只传了一个空对象，y有默认值 x等同于声明了未定义，即undefined
foo() // 报错. 因为没有传参数， 那么形参的声明的默认值就不会正确执行，即报错

/* 调用时不传参数不会报错 */
function foo({x, y = 5} = {}) {
  console.log(x, y);
}
foo()  // undefined, 5  这样就不会报错了
```

#### rest 形参

 - rest 只能在形参中的最后一位
 - 函数的length属性不包括 rest 函数

```javascript
function mypush(array, ...items) {
  items.forEach(item=>{
    array.push(item);
  });
}
var a = [];
mypush(a, 1, 2, 3); // a = [1,2,3];
```

#### 扩展运算符的应用

##### 小应用
```javascript
/*
 * 取得最大值
 */
Math.max(...[14, 3, 77])

/*
 * 将一个数组添加到另一个数组的尾部
 */
var arr1 = [0, 1, 2], arr2 = [3, 4, 5];
arr1.push(...arr2);

/*
 * 生成指定时间
 */
new Date(...[2015, 1, 1]);

/*
 * 合并数组
 */
[...arr1, ...arr2, ...arr3]

/*
 * 与解构赋值结合
 */
const [first, ...rest] = [1, 2, 3, 4, 5];
first //1
rest  //[2, 3, 4, 5]

const [first, ...rest] = [];
first //undefined
rest  //[]

const [first, ...rest] = ["foo"];
first  //"foo"
rest   //[]

/*
 * 函数的返回值
 * 从数据库取出一行数据，通过扩展运算符，直接将其传入构造函数Date。
 */
va r dateFields = readDateFields(database);
var d = new Date(...dateFields);

// 将字符串转为真正的数组
[...'hello']
// [ "h", "e", "l", "l", "o" ]

```
##### 实现了 Iterator 接口的对象
任何 Iterator 接口的对象，都可以用扩展运算符转为 <span id="inline-green">真正的数组</span>。

```javascript
var nodeList = document.querySelectorAll('div');
var array = [...nodeList];
/*
 * 扩展运算符可以将伪数组nodeList 转换真正的数组，
 * 原因就在于 NodeList 对象实现了 Iterator 接口。
 */

let arrayLike = {
  '0': 'a',
  '1': 'b',
  '2': 'c',
  length: 3
};
let arr = [...arrayLike];
// 报错
/**
 * arrayLike是一个类似数组的对象，但是没有部署Iterator接口，扩展运算符就会报错。
 * 这时，可以改为使用Array.from方法将arrayLike转为真正的数组。
 */

```

##### Map和Set结构，Generator函数

<p id="border-blue">因此只要 `具有Iterator接口的对象`，都可以使用扩展运算符</p>

```javascript
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]

```

Generator函数运行后，`返回一个遍历器对象`，因此也可以使用扩展运算符。

```javascript
var go = function*(){
  yield 1;
  yield 2;
  yield 3;
};

[...go()] // [1, 2, 3]

```
上面代码中，变量go是一个 Generator 函数，执行后返回的是一个遍历器对象，对这个遍历器对象执行扩展运算符，就会将内部遍历得到的值，转为一个数组。

----------

#### 箭头函数

##### 使用注意

 1. 箭头函数体内的 `this` 指向的是 <span id="inline-blue">父作用域</span>
 2. 不可以当作构造函数
 3. 不可以使用 arguments 对象，可以用 rest 参数代替。
 4. 不可以使用 yield 命令


##### 基本使用

```javascript
/* 一个形参 */
var f = v => v;

/* 多个形参 */
var sum = (num1, num2) => num1 + num2;

/*  由于大括号被解释为代码块，所以返回对象时要加上小括号 */
var getTempItem = id => ({ id: id, name: "Temp" });

/* 多条语句卸载大括号内 */
var sum = (num1, num2) => { return num1 + num2; }

/* 箭头函数与变量解构结合使用 */
const full = ({ first, last }) => first + ' ' + last;

// 等同于
function full(person) {
  return person.first + ' ' + person.last;
}
```

#### 绑定 this

 - 绑定 this 是用来取代call、apply、bind调用
 - 函数绑定运算符是并排的两个冒号（::）

```javascript
foo::bar;               /* 等同于 */ bar.bind(foo);

foo::bar(...arguments); /* 等同于 */ bar.apply(foo, arguments);

/* 如果双冒号左边为空，右边是一个对象的方法，则等于将该方法绑定在该对象上面。 */

var method = obj::obj.foo; /* 等同于 */ var method = ::obj.foo;
var log = ::console.log;  /* 等同于 */ var log = console.log.bind(console);

```

#### 尾调用(Tail Call)

ES6 的尾调用优化只在严格模式下开启，`正常模式是无效的`。

<p id="border-blue">尾调用是<span id="inline-purple">函数式编程</span> 的一个重要概念，就是指 <span id="yu-1">某个函数 **最后一步** 以 `return` 的方式调用另一个函数</span> </p>

```javascript
/* 尾调用 */
function f(x){
  return g(x);
}

/* 函数m和n都属于尾调用，因为它们都是函数f的最后一步操作。*/
function f(x) {
  if (x > 0) {
    return m(x)
  }
  return n(x);
}
```

**对于尾调用优化的理解**
<p id="border-purple">每函数执行时在内存中形成调用帧（保存调用位置和内部变量等信息），`调用帧的释放 取决于该函数是否执行了return` ，系统默认会自执行 return， 但前提是需要等待程序执行完毕时，那在这个过程中累积的调用帧无疑加大了内存的开销。
所以结尾处以 return 形式调用另一个函数 可以使 调用帧及时释放以减少无用的内存浪费</p>

---------------

### 对象的扩展

#### 属性的简洁表示法
```javascript
/*  对象内可以使用变量名 */
var foo = 'bar';
var baz = {foo};
baz // {foo: "bar"}

function f(x, y) {
/*内部默认执行了 x = 1,y = 2 ，所以形参也是变量*/
  return {x, y};
}
f(1, 2) // Object {x: 1, y: 2}

/* 对象内方法的简写 */
var o = {
  method() {
    return "Hello!";
  }
};
```

##### 应用场景

场景一 ：对象中直接放一个变量

```javascript
var birth = '2000/01/01';

var Person = {
  name: '张三',
  birth,
  hello() { console.log('我的名字是', this.name); }
};
Person.birth // '2000/01/01'
```

场景二 ：这种写法用于函数的返回值，将会非常方便。

```javascript
function getPoint() {
  var x = 1;
  var y = 10;
  return {x, y};
}

getPoint()
// {x:1, y:10}
```

场景三 ：CommonJS `模块输出变量` ，就非常合适使用简洁写法。

```javascript
var ms = {};

function getItem (key) {
  return key in ms ? ms[key] : null;
}

function setItem (key, value) {
  ms[key] = value;
}

function clear () {
  ms = {};
}

module.exports = { getItem, setItem, clear };
// 等同于
module.exports = {
  getItem: getItem,
  setItem: setItem,
  clear: clear
};
```

#### 属性名表达式


```javascript

/* 例子一 */
let propKey = 'foo';

let obj = {
  [propKey]: true,
  ['a' + 'bc']: 123
};
obj.foo // true
obj.abc // 123

/* 例子二 */
var lastWord = 'last word';

var a = {
  'first word': 'hello',
  [lastWord]: 'world'
};

a['first word'] // "hello"
a[lastWord] // "world"
a['last word'] // "world"

```

##### 属性表达式定义方法名

```javascript

let obj = {
  ['h' + 'ello']() {
    return 'hi';
  }
};

obj.hello() // hi

/* 属性名表达式与简洁表示法，不能同时使用 */

// 报错
var foo = 'aaa';
var aaa = 'abc';
var baz = { [foo] };

// 正确
var foo = 'aaa';
var baz = { [foo]: 'abc'};
baz.aaa // abc

```

#### Object.is()

比较俩个值是否相等,和 === 类似 .  Object.js(1,2) 等同于 1 === 2

#### Object.assign()

<p id="border-blue">Object.assign方法用于合并对象，`第一个参数是目标对象` ，后面的参数都是源对象。</p>


```javascript
var target = { a: 1 };

var source1 = { b: 2 };
var source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}

```

##### 应用场景

1. 为对象添加属性

```javascript
class Point {
  constructor(x, y) {
    Object.assign(this, {x, y});
  }
}
/* 通过Object.assign方法，将x、y属性添加到 Point 类的对象实例中 */
```

2. 为对象添加方法

```javascript

/*
 *通过 Object.assgin 函数将 aaa、bbb函数 添加到 SomeClass.prototype 原型中。
 */
Object.assign(SomeClass.prototype, {
  aaa(arg1, arg2) {
    ···
  },
  bbb() {
    ···
  }
});

// 等同于下面的写法
SomeClass.prototype.aaa = function (arg1, arg2) {
  ···
};
SomeClass.prototype.bbb = function () {
  ···
};

```
3. 合并多个对象

```javascript
const merge =
  (...sources) => Object.assign({}, ...sources);
```

4. 为属性指定默认值

```javascript
const DEFAULTS = {
  logLevel: 0,
  outputFormat: 'html'
};
/* DEFAULTS对象是默认值，options对象是用户提供的参数。 */
function processContent(options) {
  options = Object.assign({}, DEFAULTS, options);
  console.log(options);
  //  {logLevel: 0, outputFormat: "html"}
}

```

#### 属性的遍历

|方法|作用|
|----|----|
|for...in|遍历自身的和继承的可枚举属性（不含 Symbol 属性）。
|Object.keys(obj)|返回数组，（不含继承的）所有可枚举属性（不含 Symbol 属性）。
|Object.getOwnPropertyNames(obj)|返回自身对象的一个数组，不含 Symbol 属性，但是包括不可枚举属性）。
|Object.getOwnPropertySymbols(obj)|返回自身对象的一个数组，含所有 Symbol 属性。
|Reflect.ownKeys(obj)|返回自身对象的一个数组，不管属性名是 Symbol 或字符串，也不管是否可枚举。

 - 以上的5种方法遍历对象的属性，都遵守同样的属性遍历的次序规则。
   - 首先遍历 `为数值的属性`，按照 `数字排序` 。
   - 其次遍历 `字符串的属性`，按照生成时间排序。
   - 最后遍历 `Symbol 值的属性`，按照生成时间排序。

```javascript
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]
```

#### __proto__ 属性

 - Object.getprototypeOf() == 读取一个对象的原型对象
 - Object.setprototypeOf() == 设置一个对象的原型对象

#### Object.keys()/values()/entries()

<p id="border-blue">可将对象中所有的 `键或值` 单独分离进行独立出来</p>

```javascript
var obj = { foo: 'bar', baz: 42 };
Object.keys(obj)   // ["foo", "baz"]
Object.values(obj)  // ["bar", 42]
Object.entries(obj) // [ ["foo", "bar"], ["baz", 42] ]

```

#### 对象的扩展运算符

```javascript
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x // 1
y // 2
z // { a: 3, b: 4 }

/*******************************/

let obj = { a: { b: 1 } };
let { ...x } = obj;
obj.a.b = 2;
x.a.b // 2

/*******************************/
let z = { a: 3, b: 4 };
let n = { ...z };
n // { a: 3, b: 4 }

/**
 * 扩展运算符可以用于合并两个对象。
 */
let ab = { ...a, ...b };
// 等同于
let ab = Object.assign({}, a, b);
```

<span id="yu-1">扩展运算符花样操作</span>

```javascript
let aWithOverrides = { ...a, x: 1, y: 2 };
// 等同于
let aWithOverrides = { ...a, ...{ x: 1, y: 2 } };
// 等同于
let x = 1, y = 2, aWithOverrides = { ...a, x, y };
// 等同于
let aWithOverrides = Object.assign({}, a, { x: 1, y: 2 });
```
---------------

## Symbol

 - 为了从根本上解决命名冲突
 - ES6 引入了一种新的原始数据类型 Symbol，`表示独一无二的值`
 - 目前总共7种分别是: undefined、null、Nubmber、String、Boolean、Object、 <span id="inline-blue">Symbol</span>

-----------

## Set 和 Map 数据结构

###  Set

#### 基本用法

  - **类似于数组** ，但成员的值 <span id="inline-purple">没有重复</span> 的
  - 可接受 <span id="inline-blue">任何数组</span> 作为参数进行初始化
  - Set 本身是一个构造函数，用来生成 Set 数据结构。

```javascript
const set = new Set([1,1,2,3,4,4]);
[...set]
// [1, 2, 3, 4]

```

---------

#### Set 属性

|属性|作用|
|----|----|
|构造函数|Set.prototype.constructor|
|成员总数|Set.prototype.size|

---------

#### Set 方法

|操作方法|作用|
|----|----|
| add(value)：|添加某个值，返回Set结构本身。|
| delete(value)：|删除某个值，返回一个布尔值，表示删除是否成功。|
| has(value)：|返回一个布尔值，表示该值是否为Set的成员。|
| clear()：|清除所有成员，没有返回值。|


|遍历方法|作用|
|----|----|
|keys() / values()：|返回键名的遍历器|
|entries()：|返回键和值，但键和值都一样|
|forEach()：|使用回调函数遍历每个成员|

```javascript

let set = new Set(['red', 'green', 'blue']);

/* set 的默认方法就是 set.values */
for (let item of set) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]

/* forEach对每个成员执行某种操作，没有返回值。 */
let set = new Set([1, 2, 3]);
set.forEach((value, key) => console.log(value * 2) )

// 2
// 4
// 6
```

-----------

#### 应用

##### 数组去重

```javascript
//方法一
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
// [3, 5, 2]

//方法二
let arr = Array.from(new Set([1,2,2,3,4]))
// [1, 2, 3, 4]

```

##### 数组的 map 和 filter 方法结合Set轻松实现并、交、差集

```javascript
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}

```

-----------

###  Map

<p id="border-purple">解决对象的键只能是字符串的痛点（null、undefined都可以），Map 结构提供了“值和值”的对应</p>

#### 基本用法
```javascript

/* 注意是 二维数组的方式 [ [],[],[] ]*/

const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.set(o, 'content')
map.has('name') // true
map.get('name') // "张三"
map.size // 3

```

<span id="inline-red">注意</span> :Map 结构只会对 <span id="yu-1">有对象引用的</span> , 才将其视为一个键。

```javascript

const map = new Map();

map.set(['a'], 555);
map.get(['a'])

// undefined 的原因是 set 时候的 ['a'] 和 get 时候的 ['a'] 在内存地址中不是同一个位置。

/*****************************************************/

const map = new Map();

const k1 = ['a'];
const k2 = ['a'];

/* 链式写法 */
map
.set(k1, 111)
.set(k2, 222);

map.get(k1) // 111
map.get(k2) // 222

/*
  因为k1 的值是对象 ，而对象是引用类型储存在堆中，
  set 和 get 用的 都是一个引用地址，所以就能取到对应的值,
  k1 和 k2 的值虽然一样但是 存储的引用地址不一样，so 不会冲突
 */

```


#### 属性和方法

属性：**size** ，返回 Map结构成员总数。

**方法：**

|操作的|作用|
|----|----|
| set(key,value)：|设置 key 对应的键值,可采用链式写法|
| get(key)：|读取 key 对应的键值，找不到返回 undefined|
| has(value)：|返回一个布尔值，表示该值是否为 Map 的成员。|
| delete(value)：|删除某个值，返回一个布尔值，表示删除是否成功。|
| clear()：|清除所有成员，没有返回值。|


|遍历的|作用|
|----|----|
|keys() |返回键名的遍历器|
|values()|返回键值的遍历器|
|entries()：|（默认）返回键和值的遍历器|
|forEach()：|遍历map的所有成员|

<p id="border-blue">forEach方法还可以接受第二个参数，用来绑定 this。</p>

```javascript
const reporter = {
  report: function(key, value) {
    console.log("Key: %s, Value: %s", key, value);
  }
};

map.forEach(function(value, key, map) {
  this.report(key, value);
}, reporter);

```

-----------

#### 与其他数据结构的互相转换

##### Map 转为数组

>直接用扩展运算符就可以很方便的进行转化

```javascript
const myMap = new Map()
  .set(true, 7)
  .set({foo: 3}, ['abc']);
[...myMap]
// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```

##### 数组转为 Map

>直接将数组传入 Map 构造函数，就可以转化为 Map 。

```javascript
new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
// Map {
//   true => 7,
//   Object {foo: 3} => ['abc']
// }
```


##### Map 转为对象
>如果所有 Map 的键都是字符串，它可以转为对象。

```javascript
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

##### 对象转为 Map

```javascript
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// Map {"yes" => true, "no" => false}
```


##### Map 转为 JSON

>Map 转为 JSON 要区分两种情况。一种情况是，Map 的键名都是字符串，这时可以选择转为对象 JSON。

```javascript
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'

/************************************************************/

/* 另一种情况是，Map 的键名有非字符串，这时可以选择转为数组 JSON。 */
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```

##### JSON 转 Map

> JSON 转为 Map，正常情况下，所有键名都是字符串。

```javascript
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}')
// Map {'yes' => true, 'no' => false}
```


----------

## Proxy （代理）

[详细介绍](http://es6.ruanyifeng.com/#docs/proxy)

<p id="border-blue">在目标对象之前设置了一个中间人，外界访问该对象时都必须先通过这个中间人，这种机制就可以对外界的访问进行过滤和改写</p>

ES6 原声提供 Proxy 构造函数，用来生成 proxy 实例。

```javascript
var proxy = new Proxy(target, handler);
```

 - new Proxy(): 表示生成一个 `Proxy实例`
 - target :  参数表示所要 `拦截的目标对象`
 - handler: 参数也是一个对象，用来 `定制拦截行为`。

### 应用场景

<p id="border-purple">Proxy 对象可以拦截目标对象的任意属性，这使得它很合适用来写 Web 服务的客户端。
</p>

```javascript
/* 新建了一个 Web 服务的接口，这个接口返回各种数据。*/
const service = createWebService('http://example.com/data');

service.employees().then(json => {
  const employees = JSON.parse(json);
  // ···
});
/***********************************************/
/*
Proxy 可以拦截这个对象的任意属性，
所以不用为每一种数据写一个适配方法，只
要写一个 Proxy 拦截就可以了。
*/
function createWebService(baseUrl) {
  return new Proxy({}, {
    get(target, propKey, receiver) {
      return () => httpGet(baseUrl+'/' + propKey);
    }
  });
}
```

------

## Promise（承诺） 对象

### 了解 Promise

 - Promise 是异步编程的一种解决方案
 - Promise 是一个保存着一个未来才会结束的事件
 - Promise 的俩个特点
   + 只有异步操作的 <span id="inline-blue">结果</span>，才能决定当前是哪一种 <span id="line-yellow">状态</span>
     * Pending（进行中）
     * Resolved（已完成，又称 Fulfilled）
     * Rejected（已失败）
   + 一旦状态改变，就不会再变，任何时候都可以得到这个结果。
     * 状态的改变只有两种可能：从 `Pending` 变为 Resolved 和 从 `Pending` 变为 Rejected。

<p id="border-red">如果某些事件不断地反复发生，一般来说，使用 `Stream` 模式是比部署Promise更好的选择。</p>

------------


### 基本操作

<p id="border-blue">ES6 规定，Promise对象是一个 `构造函数` ，用来生成Promise实例。</p>

 - `Promise构造函数` 接受一个函数作为参数，该函数的两个参数也是函数分别是resolve和reject。
 - resolve 函数： 在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
 - reject 函数：在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。


```javascript
/* 生成 promise 实例 */
var promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});

/**
 * Promise实例生成以后，可以用then方法 分别指定Resolved状态 和 Reject状态的回调函数。
 */
promise.then(function(value) {
  // 成功时
}, function(error) {
  // 失败时 （该函数可选）
});
```

一个 Promise 对象的简单例子

![](/imgs/promise.jpg)

一个用Promise对象实现的 Ajax 操作的例子

![](/imgs/getJSON.jpg)

### Promise.prototype.then()

 - then方法是定义在原型对象Promise.prototype上的
 - then(Resolved状态回调函数,Rejected 状态的回调函数)

then方法返回的是一个 `新的` Promise实例,因此可以采用链式写法，即then方法后面再调用另一个then方法。

![](/imgs/then.jpg)


### Promise.prototype.catch()

<p id="border-blue">Promise.prototype.catch 方法是 .then(null, rejection)的别名，用于 `指定发生错误时的回调函数`。 </p>

```javascript
p.then((val) => console.log('fulfilled:', val))
  .catch((err) => console.log('rejected', err));

// 等同于
p.then((val) => console.log('fulfilled:', val))
  .then(null, (err) => console.log("rejected:", err));
```

例子

```javascript
var promise = new Promise(function(resolve, reject) {
  throw new Error('test');
});
promise.catch(function(error) {
  console.log(error);
});
/* promise抛出一个错误，就被catch方法指定的回调函数捕获。 */
// Error: test
```

俩种捕捉错误的方法

```javascript
// bad
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });

// good
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```


### 其他

[深度阅读](http://es6.ruanyifeng.com/#docs/promise)

#### Promise.all()

<span id="border-blue">Promise.all用于将多个 Promise 实例，包装成一个新的 Promise 实例。</span>

#### Promise.resolve()
<span id="border-blue">有时需要将现有对象转为Promise对象，Promise.resolve 方法就起到这个作用。</span>

#### Promise.reject()
<span id="border-blue">也会返回一个新的 Promise 实例，该实例的状态为 rejected。</span>

------

## Iterator 和 for...of循环

### Iterator 概念

 1. ES6中有四种数据集合分别是：**数组、对象(没有Iterator接口)、Map、Set**
 2. 用户如果想组合使用这些数据，就需要一种统一的接口机制，来处理所有不同的数据结构。
 3. 遍历器（Iterator）就是为各种不同的数据结构 **提供统一的访问机制的接口** 。
 4. 任何数据结构只要部署Iterator接口 ，就可以完成遍历操作。

 - Iterator的三个作用
   + 为各种数据结构，`提供一个统一访问接口` ；
   + 使得数据结构的成员能够按 `某种次序排列` ；
   + Iterator接口主要 `配合for...of` 。

### 默认的 Iterator 接口

 - 默认的Iterator接口 部署在数据结构的 `Symbol.iterator属性`
 - 也就是，一个数据结构只要具有Symbol.iterator属性，就可以认为是“可遍历的”。
 - Symbol.iterator就是 默认生成遍历器的函数,执行它就会返回一个遍历器。
 - 原生具备 Iterator 接口的数据结构：Array、Map、Set、`String`、TypedArray、函数的 arguments 对象

```javascript
  /* 具备原生 Iterator ：Array */
  let arr = ['a', 'b', 'c'];
  /*执行它就会返回一个遍历器,根本特征就是具有 next 方法 */
  let iter = arr[Symbol.iterator]();

  iter.next() // { value: 'a', done: false }
  iter.next() // { value: 'b', done: false }
  iter.next() // { value: 'c', done: false }
  iter.next() // { value: undefined, done: true }
```

 ![](/imgs/symbol.jpg)


### 调用 Iterator 接口的场合



```javascript
/**
 * 1.解析赋值
 */
let [first, ...rest] = ['a','b','c'];
// first='a'; rest=['b','c'];

/**
 * 2.扩展运算符
 */
let arr = ['b', 'c'];
['a', ...arr, 'd']
// ['a', 'b', 'c', 'd']

/**
 * 3. yield*
 * yield*后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。
 */
let generator = function* () {
  yield 1;
  yield* [2,3,4];
  yield 5;
};

var iterator = generator();

iterator.next() // { value: 1, done: false }
iterator.next() // { value: 2, done: false }
iterator.next() // { value: 3, done: false }
iterator.next() // { value: 4, done: false }
iterator.next() // { value: 5, done: false }
iterator.next() // { value: undefined, done: true }
```

### 遍历器对象的 return(),throw()

遍历器对象具有next、\_return、\_throw方法。

return方法的使用场合是，`如果for...of循环提前退出`（通常是因为出错，或者有break语句或continue语句），就会调用return方法。

### for...of 循环

for...of循环 `内部调用` 的是数据结构的`Symbol.iterator方法`，所以只要数据结构只要部署了Symbol.iterator属性，就可以用for...of循环遍历它的成员


### 与其他遍历语法比较

 - for循环
   + 没有 fo...of 简洁
 - forEach
   + 无法中途跳出forEach循环，break命令或return命令都不能奏效
 - for...in
   + 主要是为遍历对象而设计的，不适用于遍历数组

## Generator 函数的语法

### 基本概念

Generator 函数是 ES6 提供的一种 `异步编程解决方案` ，语法行为与传统函数 `完全不同` 。

 - Generator 函数有多种理解角度。
   + 从语法上，首先可以把它理解成，`Generator 函数是一个状态机`，封装了多个内部状态。
   + 同时执行 Generator 函数会 `返回一个遍历器对象`
   + 返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

 - Generator 函数形式上的两个特征。
   + function 与函数名的中间有一个星号；
   + 函数体内部使用yield表达式，定义不同的内部状态（ yield 在英语里的意思就是“产出”）。

 ![](/imgs/Generator.jpg)



#### yield 表达式

<p id="border-blue">由于 Generator 函数返回的遍历器对象，只有调用next方法才会遍历下一个内部状态，所以其实提供了一种可以 `暂停执行的函数`,yield表达式就是暂停标志。</p>

 - 遍历器对象的next方法的运行逻辑如下。
   1. 遇到yield表达式，暂停并将 `紧跟在yield后面`的那个 `表达式的值` ，作为返回的对象的value属性值。
   2. 下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。
   3. 如果没有再遇到新的yield表达式，就一直运行到return语句为止，并以对象行使返回return语句后面的表达式的值
   4. 如果该函数没有return语句，则返回的对象的value属性值为undefined。

 - yield 于 return 的区别
   + yield 函数暂停执行，下一次再从该位置继续向后执行，具备记忆
   + 一个函数里面，只能执行一次return语句，但是可以执行多个yield表达式。
   + 正常函数只能返回一个值，因为只能执行一次return；
   + 函数可以返回一系列的值，因为可以有任意多个yield。

yield表达式如果用在另一个表达式之中，必须放在圆括号里面。

```javascript
function* demo() {
  console.log('Hello' + yield); // SyntaxError
  console.log('Hello' + yield 123); // SyntaxError

  console.log('Hello' + (yield)); // OK
  console.log('Hello' + (yield 123)); // OK
}
```

### next 方法的参数

<p id="border-blue">next方法的参数 表示 `上一个yield表达式的返回值` </p>

```javascript
/* 简单例子 */
  function* f() {
    for(var i = 0; true; i++) {
      var reset = yield i;
      if(reset) { i = -1; }
    }
  }

  var g = f();

  g.next() // { value: 0, done: false }
  g.next() // { value: 1, done: false }

  /*
  当next方法带一个参数true时，
  变量reset就被重置为这个参数（即true），
  因此i会等于-1，下一轮循环就会从-1开始递增。
  */
  g.next(true) // { value: 0, done: false }

/* 再看一个通过next方法的参数，向 Generator 函数内部输入值的例子 */
  function* dataConsumer() {
    console.log('Started');
    console.log(`1. ${yield}`);
    console.log(`2. ${yield}`);
    return 'result';
  }

  let genObj = dataConsumer();
  genObj.next();
  // Started
  genObj.next('a')
  // 1. a
  genObj.next('b')
  // 2. b
```

### Generator.prototype.throw()

<p id="border-blue">Generator 函数返回的遍历器对象，都有一个throw方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。</p>

### Generator.prototype.return()

<p id="border-blue">Generator函数返回的遍历器对象，还有一个return方法，可以`返回给定的值`，并且终结遍历Generator函数。</p>

### yield* 表达式

如果在 Generator 函数内部，调用另一个 Generator 函数，默认情况下是没有效果的。

```javascript
function* inner() {
  yield 'hello!';
}

function* outer2() {
  yield 'open'
  yield* inner()
  yield 'close'
}

var gen = outer2()
gen.next().value // "open"
gen.next().value // "hello!"
gen.next().value // "close"

```

### 作为对象属性的 Generator 函数

```javascript
/* myGeneratorMethod属性前面有一个星号，表示这个属性是一个 Generator 函数。 */
  let obj = {
    * myGeneratorMethod() {
      // ···
    }
  };

/* 与上面的写法是等价的 */
  let obj = {
    myGeneratorMethod: function* () {
       // ···
    }
  };

```

### 应用场景

 - 异步操作的同步化表达
 - 控制流管理
 - 部署 Iterator 接口
 - 作为数据结构

---------

## Generator 函数的异步应用

<p id="border-blue">异步编程对 JavaScript 语言太重要,因为 Javascript 语言的执行环境是 "单线程"的.
</p>

[详细阅读](http://es6.ruanyifeng.com/#docs/generator-async)

**async 函数**

 一句话，它就是 Generator 函数的语法糖

[详细阅读](http://es6.ruanyifeng.com/#docs/async)

------

## Class 的基本语法

ES6 的class写法只是让对象原型的 `写法更加清晰` 、`更接近主流面向对象编程` 的语法而已。
![](/imgs/es6_1.jpg)

[深度阅读](http://es6.ruanyifeng.com/#docs/class)

[Class的继承](http://es6.ruanyifeng.com/#docs/class-extends)

---------

## Module 的语法

### 概述

 - 历史上，JavaScript 一直没有模块体系，直接导致了对开发大型的、复杂的项目造成了巨大障碍。
 - ES6 实现了 `模块功能` ，而且实现得 `相当简单` ，完全可以取代其他规范。

<p id="border-blue">ES6 模块的设计思想`是尽量的静态化`，使得 `编译时` 就能 `确定` 模块的 `依赖关系` ，以及输入和输出的变量。</p>

**CommonJS 与 ES6 模块的区别**

![](/imgs/Module.jpg)

--------

### 严格模式

<p id="border-yellow">ES6 的模块自动采用严格模式，不管你有没有在模块头部加上 `"use strict"`</p>

 - 严格模式主要有以下限制
   + 变量必须声明后再使用
   + 函数的参数不能有同名属性，否则报错
   + 不能使用 `with` 语句
   + 不能对只读属性赋值，否则报错
   + 不能使用前缀0表示八进制数，否则报错
   + 不能删除不可删除的属性，否则报错
   + 不能删除变量 `delete prop`，会报错，只能删除属性 `delete global[prop]`
   + `eval` 不会在它的外层作用域引入变量
   + `eval` 和 `arguments` 不能被重新赋值
   + arguments不会自动反映函数参数的变化
   + 不能使用 `arguments.callee`
   + 不能使用 `arguments.caller`
   + 禁止 `this` 指向全局对象
   + 不能使用 `fn.caller` 和 `fn.arguments` 获取函数调用的堆栈
   + 增加了保留字（比如 `protected` 、`static` 和 `interface` ）

--------

### export 命令

<p id="border-blue">模块功能主要由两个命令构成：`export` 命令用于 `规定` 模块的 `对外接口` ，`import` 命令用于 `输入其他模块提供的功能`。</p>

#### 输出变量

 - 一个模块就是一个独立的文件，其内部所有变量 `外部无法获取`。
 - 如果希望外部能够读取模块内部的某个变量，就必须使用 `export输出该变量`。

```javascript
// profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;

/* 上下写法等同 */

// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};

```

#### 输出函数或类

```javascript
 export function multiply(x, y) {
  return x * y;
};
// 输出 multiply 函数
```

#### 使用as关键字重命名


```javascript
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
/* 使用as关键字，重命名了函数v1和v2的对外接口。重命名后，v2可以用不同的名字输出两次。*/
```

#### 特别注意

<p id="border-yellow">`export` 规定是: `对外的接口` 与 `模块内部的变量` 建立一一 `对应关系` 。</p>

![](/imgs/lookOut.jpg)


----

### import 命令

<p id="border-blue">使用export命令定义了模块的对外接口以后，其他 JS 文件就可以通过import命令加载这个模块。</p>

```javascript
/**
 * 1.import命令，用于加载profile.js文件，并从中输入变量。
 * 2.import命令接受一对大括号，里面指定要从其他模块导入的变量名。
 * 3.大括号里面的变量名，必须与被导入模块（profile.js）对外接口的名称相同。
 */
// main.js
import {firstName, lastName, year} from './profile';

/* import命令要使用 as 关键字，将输入的变量重命名。*/
// import { lastName as surname } from './profile';

function setName(element) {
  element.textContent = firstName + ' ' + lastName;
}
```

 - 知识点
   + from指定模块文件的位置时可以是相对或绝对路径，.js可以省略
   + 如果只是模块名，不带有路径，那么必须有配置文件，告诉 JS 引擎该模块的位置。
   + import命令具有提升效果，会提升到整个模块的头部，首先执行。

---------

### 模块的整体加载

整体加载：即用星号（*）`指定一个对象`，所有输出值都加载在这个对象上面。

```javascript

/* 模块的整体加载 */
import * as circle from './circle';

console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));

/* 上下同等 */

import { area, circumference } from './circle';

console.log('圆面积：' + area(4));
console.log('圆周长：' + circumference(14));
```

### export default 命令

#### 为模块指定默认输出

```javascript
// export-default.js
export default function () {
  console.log('foo');
}

/**
 * 模块文件export-default.js，它的默认输出是一个函数,
 * 其他模块加载该模块时，import命令可以为该匿名函数指定任意名字，不需要大括号包裹
 */

// import-default.js
import customName from './export-default';
customName(); // 'foo'
```

#### 比较默认输出和正常输出

```javascript
// 第一组
export default function crc32() { // 输出
  // ...
}

import crc32 from 'crc32'; // 输入

/********************************************/

// 第二组
export function crc32() { // 输出
  // ...
};

import {crc32} from 'crc32'; // 输入
```

本质上，export default就是输出一个叫做default的变量或方法，然后系统允许你为它取任意名字。所以，下面的写法是有效的。

```javascript
// modules.js
function add(x, y) {
  return x * y;
}
export {add as default};
// 等同于
// export default add;

// app.js
import { default as xxx } from 'modules';
// 等同于
// import xxx from 'modules';


```

有了export default命令，输入模块时就非常直观了，以输入 lodash 模块为例。

```javascript
/* 将默认方法 赋值到 _*/
import _ from 'lodash';

/* 同时输入默认方法和其他接口 */
import _, { each, each as forEach } from 'lodash';

// 对应上面代码的export语句如下。
export default function (obj) {
  // ···
}

export function each(obj, iterator, context) {
  // ···
}

export { each as forEach }; // 暴露出forEach接口，默认指向each接口，即forEach和each指向同一个方法。

```


### export 与 import 的复合写法

<p id="border-pruple">如果在一个模块之中，先输入后输出同一个模块，import语句可以与export语句写在一起。</p>

```javascript
/****************************************/
// export和import语句可以结合在一起
export { foo, bar } from 'my_module';

// 等同于
import { foo, bar } from 'my_module';
export { foo, bar };

/****************************************/

// 接口改名
export { foo as myFoo } from 'my_module';

// 整体输出
export * from 'my_module';

// 默认接口的写法
export { default } from 'foo';

/****************************************/
// 具名接口改为默认接口的写法

export { es6 as default } from './someModule';

// 等同于
import { es6 } from './someModule';
export default es6;
/****************************************/
// 默认接口也可以改名为具名接口
export { default as es6 } from './someModule';
```

-------

### 模块的继承

假设有一个circleplus模块，继承了circle模块。

```javascript

// circleplus.js 输出文件
export * from 'circle';       // 表示再输出circle模块的所有属性和方法
export var e = 2.71828182846; // 又输出了自定义的e变量和默认方法。
export default function(x) {  // 输出默认方法
  return 'test';
}
export { area as circleArea } from 'circle'; // 只输出circle模块的area方法，且将其改名为circleArea。

//main.js 输入文件
import * as math from 'circleplus'; // 加载 circleplus 上所有方法
import aaa from 'circleplus';       // 将circleplus的默认方法命名为 aaa
console.log(aaa());                 // test

```

-------

### 跨模块常量

设置跨模块的常量，或者说一个值要被多个模块共享，可以采用下面的写法。

```javascript
// constants.js 模块
export const A = 1;
export const B = 3;
export const C = 4;

// test1.js 模块
import * as constants from './constants';
console.log(constants.A); // 1
console.log(constants.B); // 3

// test2.js 模块
import {A, B} from './constants';
console.log(A); // 1
console.log(B); // 3
```

如果要使用的常量非常多，可以建一个专门的constants目录，将各种常量写在不同的文件里面，保存在该目录下。

```javascript
// constants/db.js
export const db = {
  url: 'http://my.couchdbserver.local:5984',
  admin_username: 'admin',
  admin_password: 'admin password'
};

// constants/user.js
export const users = ['root', 'admin', 'staff', 'ceo'];

/******************************************************/
// constants/index.js
export {db} from './db';
export {users} from './users';

/******************************************************/
// 使用的时候，直接加载 出入该模块，会自动加载其中的 index.js 文件

// script.js
import {db, users} from './constants';

```

-----------

### import()

 - 只是有一个 <span id="inline-yellow">提案</span>，建议引入import()函数，完成动态加载。
 - import()类似于 Node 的require方法，区别主要是前者是异步加载，后者是同步加载。
 - 适用场合
   + 按需加载
   + 条件加载
   + 动态的模块路径

----------

## Module 的加载实现

### 浏览器加载

**传统方法**

```
<!-- 页面内嵌的脚本 -->
<script type="application/javascript">
  // module code
</script>

<!-- 外部脚本 -->
<script type="application/javascript" src="path/to/myModule.js">
</script>

<!-- 异步加载的俩种方式 -->
<script src="path/to/myModule.js" defer></script> // 等到整个页面正常渲染结束，才会执行
<script src="path/to/myModule.js" async></script> // 一旦下载完成，立马执行
```

**加载规则**

浏览器加载 ES6 模块，也使用 `<script>` 标签，但是要加入 `type="module"` 属性。

```
<script type="module" src="foo.js"></script>
浏览器对于带有type="module" 都是执行 defer属性的异步加载
```

[与Node相关的ES6](http://es6.ruanyifeng.com/#docs/module-loader#浏览器加载)

---------

## 编程风格

### 块级作用域

 - `let` 完全取代var
 - let 和 const 之间，优先使用 `const`

### 字符串

静态字符串一律使用 `单引号或反引号`，不使用双引号

```javascript
// bad
const a = "foobar";
const b = 'foo' + a + 'bar';

// good
const a = 'foobar';
const b = `foo${a}bar`;
const c = 'foobar';
```

### 解构赋值

```javascript
const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;

```

### 对象

 - 单行定义的对象，最后一个成员 `不以逗号结尾`。
 - 多行定义的对象，最后一个成员 `以逗号结尾`。

```javascript
// bad
const a = { k1: v1, k2: v2, };
const b = {
  k1: v1,
  k2: v2
};

// good
const a = { k1: v1, k2: v2 };
const b = {
  k1: v1,
  k2: v2,
};
```

### 数组

使用扩展运算符 (...) 拷贝数组

```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```

使用Array.from方法，将伪数组转为真数组。

```javascript
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```

### 函数


```javascript
/**
 * 立即执行函数可以写成箭头函数的形式。
 */
(() => {
  console.log('Welcome to the Internet.');
})();

/**
 * 需要使用函数表达式的场合，尽量用箭头函数代替
 */
[1, 2, 3].map((x) => {
  return x * x;
});

// 或者
[1, 2, 3].map(x => x * x);

/**
 * 所有配置项都应该集中在一个对象，放在最后一个参数，布尔值不可以直接作为参数
 */
function divide(a, b, { option = false } = {}) {
}

/**
 * 使用rest运算符（...）代替 arguments
 */
function concatenateAll(...args) {
  return args.join('');
}

/**
 * 设置形参默认值。
 */
function handleThings(opts = {}) {
  // ...
}

```

### Map 结构

 - **注意区分 Object 和 Map**。
 - 如果只是需要key: value的数据结构，使用Map结构。

### Class

用Class 取代需要 prototype 的操作，因为Class的写法更简洁，更易于理解。

![](/imgs/es6_1.jpg)

### 模块

 - Module 语法是 JS 模块的 标准写法 ，坚持使用这种写法。
   + 使用 `import` 取代 require。
   + 使用 `export` 取代 module.exports。

 - 注意事项
   + 如果模块只有一个输出值，才使用export default。
   + 在模块输入中使用通配符，就无法确保有一个默认输出值
   + 模块默认输出一个函数，`函数名首字母小写`。
   + 模块默认输出一个对象，`对象名首字母大写`。

