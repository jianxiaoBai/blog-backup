---
title: python
date: 2018-10-05 13:20:53
categories: [技术笔记]
tags: [vim, 基本操作, 笔记]
comments: false
---

- #开头，注释
- 缩进的语句视为代码块
- 大小写敏感

## 数据类型和变量

- True 和 False 首字母大写
- and、or 和 not
- 空值：None

## 字符串格式化%

- %d 整数
- %f 浮点数
- %s 字符串
- %x 十六进制整数
- 补位

<!-- more -->

```py
>>> '%2d-%02d' % (3, 1)
' 3-01'
>>> '%.2f' % 3.1415926
'3.14'
```

## 数组：list 和 tuple

- list 数组
  - len(list)得到长度
  - list[-2]获得倒数第二个元素
  - list.append(ele)往 list 中追加元素到末尾
  - list.insert(1, ele)，把元素插入到指定的位置，比如索引号为 1 的位置
  - list.pop()，删除 list 末尾的元素，用 pop()方法
  - list.pop(i)删除指定位置的元素，用 pop(i)方法，其中 i 是索引位置
  - 元素的数据类型也可以不同，L = ['Apple', 123, True]
- tuple 数组：classmates = ('Michael', 'Bob', 'Tracy')
  - tuple 一旦初始化就不能修改，代码更安全

## 条件判断和循环

- if elif else
- for in
- while
- range()函数
  ```py
  >>> range(1,5) #代表从1到5(不包含5)
  [1, 2, 3, 4]
  >>> range(1,5,2) #代表从1到5，间隔2(不包含5)
  [1, 3]
  >>> range(5) #代表从0到5(不包含5)
  [0, 1, 2, 3, 4]
  ```
- raw_inpit(str)读取的内容永远以字符串的形式返回

## dict 和 set

- dict 就是 map，用 key-value 的形式存储。

```py
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
```

- 根据 key 获得 value
  - []：一旦 key 不存在就会报错
  - get()函数：如果 key 不存在，可以返回 None，或者自己指定的 value（作为第二个参数传入）
  - 'Thomas' in d 如果不存在则返回 False
- set 是一组 key 的集合，但不存储 value，key 不能重复。
  - 需要 list 作为输入
  - add(key)函数用来往里面添加元素，自动忽略重复
  - remove(key)函数用来删除元素
  - &操作用来做交集
  - |操作用来做并集

## 函数

[所有内置函数](https://docs.python.org/2/library/functions.html)

- 类型检查 `isinstance`
- 可变参数 **\***
- 关键字参数 \*\* ,
- 参数定义的顺序必须

### 默认参数

```py
def power(x, n = 2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
```

> **定义默认参数要牢记一点：默认参数必须指向不变对象！**

### 可变参数

```py
a = [2, 3, 5]
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
calc(2, 3, 5) # 10
calc(*a) # 10
```

### 关键参数

```py
extra = {'city': 'Beijing', 'job': 'Engineer'}

def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
>>> person('Michael', 30)
# name: Michael age: 30 other: {}
>>> person('Adam', 45, gender = 'M', job = 'Engineer')
# name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
>>> person('Jack', 24, city = extra['city'], job = extra['job'])
# name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
# name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

> kw 获得的 dict 是 extra 的一份拷贝，对 kw 的改动不会影响到函数外的 extra。

### 命名关键字参数

`*号` 后面的参数被视为 **命名关键字参数**

```py
def person(name, age, *, city = 'shanghai', job):
    # 没有 city 和 job 字段或多了其他字段则会报错
    print(name, age, city, job)

person('Jack', 24, city='Beijing', job='Engineer')
# Jack 24 Beijing Engineer
```

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符 \* 了：

```py
def person(name, age, *args, city, job):
    print(name, age, args, city, job)

person('张三', 12, 213, city='北京', job='frontEnd')
# 张三 12 (12, 213) 北京 frontEnd
```

> 如果没有 **可变参数**，就必须加一个 `*` 作为特殊分隔符。如果缺少\*，Python 解释器将无法识别 **位置参数** 和 **命名关键字参数**

### 参数组合

在 Python 中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这 5 种参数都可以组合使用。

但是请注意，参数定义的顺序必须是：

1. 必选参数
2. 默认参数 `x = 5`
3. 可变参数 `*num`
4. 命名关键字参数 `**ak`
5. 关键字参数 `*` (如存在可变参数则不需声明)

### 定义空函数

```py
def nop():
    pass
```

> _pass 可以用来作为占位符_

## 高级特性

### 切片（Slice ）

```py
>>> L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
>>> L[0:3]
['Michael', 'Sarah', 'Tracy']
```

- `L[0:3]` 表示，从索引 0 开始取，直到索引 3 为止，但不包括索引 3。即索引 0，1，2，正好是 3 个元素。
- 如果第一个索引是 0，还可以省略。
- `L[-1]` 取倒数第一个元素，也支持倒数切片：L(-2:)
- `只写[:]` 就可以原样复制一个 list
- `L[:10:2]` 表示前十个元素，每两个取一个：[0,2,4,6,8]
- `L[:10:2]` 前 10 个数，每两个取一个
- `L[::5]` 所有数，每 5 个取一个：
- `tuple` 也可以用切片，操作结果也是 tuple
- 字符串也支持切片

### 迭代（Iteration）

- 只要是可迭代对象（list，tuple，dict，set，字符串）都可以用 for...in...迭代
- 默认情况下，dict 迭代的是 key。
  - 如果要迭代 value，可以用 for value in d.itervalues()
  - 如果要同时迭代 key 和 value，可以用 for k, v in d.iteritems()。
- 判断一个对象是否是可迭代对象：

```py
from collections import Iterable
isinstance('abc', Iterable) # str是否可迭代 True
isinstance([1,2,3], Iterable) # list是否可迭代 True
isinstance(123, Iterable) # 整数是否可迭代 False
```

**拥有下标的循环：**

```py
for i, value in enumerate(['A', 'B', 'C']):
  print i, value
```

**for 循环同时引用两个变量：**

```py
for x, y in [(1, 1), (2, 4), (3, 9)]:
	print x, y
```

### 列表生成式（List Comprehensions）

- [x * x for x in range(1, 11)] => [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
- 加上判断：[x * x for x in range(1, 11) if x % 2 == 0] => [4, 16, 36, 64, 100]
- 两层循环（可以用来生成`全排列`）：[m + n for m in 'ABC' for n in 'XYZ'] => ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

### 生成器（Generator）

> 在 Python 中，这种一边循环一边计算的机制，称为生成器：generator。

生成器里面装了用来生成一个 list 的`算法`，这样就不必创建完整的 list，从而大量的`节省空间`。

- 如何创建 generator
  - 把列表生成的 [] 改成 ()
  - 函数内使用 `yield`

```py
g = (x * x for x in range(3))
g.next() # 0
g.next() # 1
g.next() # 4
g.next() # StopIteration

for n in g:
  print n
```

定义 generator 的另一种方法，yield：

```py
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```

> 如果一个函数定义中包含 `yield` 关键字，那么这个函数就不再是一个普通函数，而是一个 `generator`

可以直接使用 for 循环来迭代, 但是那样获取不到返回值, 必须使用捕获 `StopIteration` 错误, 返回值包含在 StopIteration 的 value 中：

```py
g = fib(6)
while True:
     try:
         x = next(g)
         print('g:', x)
     except StopIteration as e:
         print('Generator return value:', e.value)
         break
# g: 1
# ...
# Generator return value: done
```

#### Generator 的执行顺序

`generator` 函数在每次调用 next()时时候执行到 `yield` 语句返回，再次执行时从上次返回的 yield 语句处继续执行。

```py
def odd():
    print('step 1')
    yield 1
    print('step 2')
    yield(3)
    print('step 3')
    yield(5)
o = odd()
>>> next(o)
# step 1
# 返回值 1
>>> next(o)
# step 2
# 返回值 3
>>> next(o)
# step 3
# 返回值 5
```

### 迭代器

- 凡是可作用于 for 循环的对象都是 Iterable 类型；
- 凡是可作用于 next()函数的对象都是 Iterator 类型，它们表示一个惰性计算的序列；
- list => []、dict => {}、str = 'aaa' 是 **Iterable** 但不是 **Iterator**
- 非 **Iterator** 可以通过 `iter()` 函数获得一个该对象。

## 模块

```
mycompany
├─ __init__.py
├─ abc.py
└─ xyz.py
```

引入了包以后，只要顶层的包名不与别人冲突，那所有模块都不会与别人冲突。现在，abc.py 模块的名字就变成了 mycompany.abc，类似的，xyz.py 的模块名变成了 mycompany.xyz。

每一个包目录下面都 **必须有一个** `__init__.py` 的文件，否则，Python 就把这个目录当成普通目录，而不是一个包。`__init.py__` 可以是空文件，也可以有 Python 代码，因为`__init.py__` 本身就是一个模块，而它的模块名就是 mycompany。

类似的，可以有多级目录，组成多级层次的包结构。比如如下的目录结构：

```
mycompany
 ├─ web
 │  ├─ __init__.py
 │  ├─ utils.py
 │  └─ www.py
 ├─ __init__.py
 ├─ abc.py
 └─ xyz.py
```

文件 www.py 的模块名就是 mycompany.web.www，两个文件 utils.py 的模块名分别是 mycompany.utils 和 mycompany.web.utils。

模块是一组 Python 代码的集合，可以使用其他模块，也可以被其他模块使用。

- 创建自己的模块时，要注意：
  - 模块名要遵循 Python 变量命名规范，不要使用中文、特殊字符；
  - 模块名不要和系统模块名冲突，最好先查看系统是否已存在该模块，检查方法是在 Python 交互环境执行 import abc，若成功则说明系统存在此模块。

### 模块模板

```py
#!/usr/bin/env python3  # 可以让这个hello.py文件直接在Unix/Linux/Mac上运行
# -*- coding: utf-8 -*- # 文件本身使用标准UTF-8编码；
' a test module ' # 任何模块代码的第一行字符串都被视为模块的文档注释；
__author__ = 'Michael Liao' # 把作者写进去，这样当你公开源代码后别人就可以瞻仰你的大名

# 以上就是Python模块的标准文件模板

import sys

def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')
# 当我们在 命令行运行 hello 模块文件时，Python 解释器把一个特殊变量 __name__ 置为 __main__
# 也就是下面的这个 if 只有用命令行运行才会执行
if __name__=='__main__':
    test()
```

**作用域**

- 类似`__xxx__`这样的变量是特殊变量，可以被直接引用，但是有特殊用途，我们自己的变量一般不要用这种变量名；
- 类似`_xxx` 和 `__xxx__` 这样的函数或变量就是非公开的（private），不应该被直接引用

private 函数或变量不应该被别人引用，那它们有什么用呢？请看例子：

```py
def _private_1(name):
    return 'Hello, %s' % name

def _private_2(name):
    return 'Hi, %s' % name

def greeting(name):
    if len(name) > 3:
        return _private_1(name)
    else:
        return _private_2(name)
```

> 外部不需要引用的函数全部定义成 `private`，只有外部需要引用的函数才定义为 `public`

## 面向对象编程

未完待续...