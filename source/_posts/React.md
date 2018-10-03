---
title: React 笔记
date: 2017-07-10 10:17:19
tags: [React,笔记]
keywords: [React,React笔记,阮一峰]
categories: [技术笔记]
comments: false
---


## 注意点

 - 注意区分大小写
 - 单标签后面一定要有 `/` 闭合符

<!-- more -->

## ReactDOM.render()

 React 的最基本方法，用于 将模板转为 HTML 语言，并插入到指定的 DOM 节点。

## JSX 语法

 - JSX 的基本语法规则：
     + 遇到 HTML 标签（以 < 开头），就用 HTML 规则解析；
     + 遇到代码块（以 { 开头），就用 JavaScript 规则解析;
     + JSX 允许直接在模板插入 JS 变量 `{x}`。
     + 如果这个变量是一个数组，则会展开这个数组的所有成员

## 组件

React 允许将代码封装成组件（component），然后像插入普通 HTML 标签一样，在网页中插入这个组件。

 - 组建类的第一个字母必须大写,否则会报错
 - 组件类只能包含一个顶层标签,否则也会报错
 - 组件可以任意加属性
     + 比如 `<HelloMessage name="John">`
     + 组件的属性可以在组件类的 `this.props `对象上获取 比如 `this.props.John`
         * 添加组件属性，保留字需要注意就是
         * class 属性需要写成 className
         * for 属性需要写成 htmlFor

## this.props.children

 - `this.props` 对象的属性与组件的属性一一对应.
 - `this.props.children` 属性,表示组件的所有子节点

React.Children.map 是专门来遍历 this.props.children 的一个方法

## PropTypes

 - 组件的属性可以接受任意值，字符串、对象、函数等等都可以。
 - 有时，我们需要一种机制，验证别人使用组件时，提供的参数是否符合要求。
 - `getDefaultProps` 方法可以用来设置组件属性的默认值。

## 获取真实的DOM节点

 - 组件是存在于内存之中的一种数据结构，叫做虚拟(virtual) DOM,只有当它插入文档以后，才会变成真实的 DOM 。
 - React 中所有的 DOM 变动之前都先在虚拟 DOM 上发生，然后再将 `实际发生变动` 的部分，反映在真实DOM上，这种算法叫做 `DOM diff` ，它可以极大提高网页的性能表现。

但是，有时需要从组件获取真实 DOM 的节点，这时就要用到 ref 属性.

```javascript
var MyComponent = React.createClass({
  handleClick: function() {
    this.refs.myTextInput.focus();
  },
  render: function() {
    return (
      <div>
        <input type="text" ref="myTextInput" />
        <input type="button" value="Focus the text input" onClick={this.handleClick} />
      </div>
    );
  }
});

ReactDOM.render(
  <MyComponent />,
  document.getElementById('example')
);

```

 - 组件 `MyComponent` 的子节点有一个文本输入框，用于获取用户的输入。
 - 这时就必须获取真实的 DOM 节点，`虚拟 DOM 是拿不到用户输入的`。
 - 为了做到这一点，文本输入框 `必须有一个 ref 属性`，然后 this.refs.[refName] 就会返回这个真实的 DOM 节点。
 - 由于 this.refs.[refName] 属性获取的是 `真实 DOM` ，所以`必须等到虚拟 DOM 插入文档以后`，才能使用这个属性，否则会报错。
 - 上面代码中，通过为组件指定 Click 事件的回调函数，确保了只有等到真实 DOM 发生 Click 事件之后，才会读取 this.refs.[refName] 属性。


## this.state

React 中将组件看成是一个状态机，开始有一个初始状态，然后用户互动，导致状态变化，从而触发重新渲染 UI

```javascript
var LikeButton = React.createClass({
  getInitialState: function() {
    return {liked: false};
  },
  handleClick: function(event) {
    this.setState({liked: !this.state.liked});
  },
  render: function() {
    var text = this.state.liked ? 'like' : 'haven\'t liked';
    return (
      <p onClick={this.handleClick}>
        You {text} this. Click to toggle.
     </p>
    );
  }
});

ReactDOM.render(
  <LikeButton />,
  document.getElementById('example')
);

```

 - `LikeButton` 组件中的 `getInitialState` 方法用于定义初始状态，也就是一个对象，这个对象可以通过 this.state 属性读取。
 - 当用户点击组件，导致状态变化，`this.setState` 方法就修改状态值，每次修改以后，自动调用 this.render 方法，再次渲染组件。
 - 由于 this.props 和 this.state 都用于描述组件的特性，可能会产生混淆。
    - this.props 表示那些一旦定义，就不再改变的特性
    - this.state 是会随着用户互动而产生变化的特性


## 表单

类似于双向数据绑定

```javascript

var Input = React.createClass({
  getInitialState: function() {
    return {value: 'Hello!'};
  },
  handleChange: function(event) {
    this.setState({value: event.target.value});
  },
  render: function () {
    var value = this.state.value;
    return (
      <div>
        <input type="text" value={value} onChange={this.handleChange} />
       <p>{value}</p>
      </div>
    );
  }
});

ReactDOM.render(<Input/>, document.body);

```

文本输入框的值，不能用 this.props.value 读取，而要定义一个 `onChange` 事件的回调函数，通过 `event.target.value` 读取用户输入的值。


## 组建的生命周期

 - 组件周期的三个状态
     + Mounting：已插入真实 DOM
     + Updating：正在被重新渲染
     + Unmounting：已移出真实 DOM

 - React 为每个状态都提供了两种处理函数，`will 函数在进入状态之前调用`，`did 函数在进入状态之后调用`
     + componentWillMount()
     + componentDidMount()
     + componentWillUpdate(object nextProps, object nextState)
     + componentDidUpdate(object prevProps, object prevState)
     + componentWillUnmount()

 - 此外，React 还提供两种特殊状态的处理函数。
     + componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用
     + shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用

---------

参考自:[阮一峰入门实例教程](http://www.ruanyifeng.com/blog/2015/03/react.html)
