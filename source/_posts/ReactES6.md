---
title: RN 中 ES6 书写规范
date: 2017-07-25 10:23:31
tags: [React,书写规范,ES6]
categories: [技术笔记]
keywords: [React,书写规范]
comments: false
---

## 前言

<p id="border-blue">很多时候我们学会了很多东西,但是不知道 `怎么用、如何用、在哪用`这时这篇文章就显得 `很有用` 了。
参考依照：[ES5 ES6写法对照表](http://bbs.reactnative.cn/topic/15/react-react-native-%E7%9A%84es5-es6%E5%86%99%E6%B3%95%E5%AF%B9%E7%85%A7%E8%A1%A8)</p>

------

<!-- more -->


## 模块

### 引用

```js
import {
    Image,
    Text
} from 'react-native'

```

### 导出单个类

```js
/* 导出 */
export default class MyComponent extends Component{
    ...
}

/* 引入 */
import MyComponent from './MyComponent';
```

---------


## 组件

### 定义组件

```js
class Photo extends React.Component {
    render() {
        return (
            <Image source={this.props.source} />
        );
    }
}

```

### 给组件定义方法

```js
class Photo extends React.Component {
    componentWillMount() {

    }
    render() {
        return (
            <Image source={this.props.source} />
        );
    }
}
```


### 定义组件的 `属性类型` 和 `默认属性`

在ES6里，可以统一使用 `static` 成员来实现

```js
class Video extends React.Component {
    static defaultProps = {
        autoPlay: false,
        maxLoops: 10,
    };  // 注意这里有分号
    static propTypes = {
        // 定义 autoPlay必须是 布尔值
        autoPlay: React.PropTypes.bool.isRequired,
        maxLoops: React.PropTypes.number.isRequired,
        posterFrameSrc: React.PropTypes.string.isRequired,
        videoSrc: React.PropTypes.string.isRequired,
    };  // 注意这里有分号
    render() {
        return (
            <View />
        );
    } // 注意这里既没有分号也没有逗号
}
```

--------


## 初始化 STATE


第一种写法：方便、简单

```js

class Video extends React.Component {
    state = {
        loopsRemaining: this.props.maxLoops,
    }
}
```


第二种写法：语法上易理解，可以根据需要做一些计算

```js
class Video extends React.Component {
    constructor(props){
        super(props);
        this.state = {
            loopsRemaining: this.props.maxLoops,
        };
    }
}

```

-------

## 把方法作为 `回调`

在ES6下，需要通过bind来绑定this引用，或者使用箭头函数来调用

```js
class PostInfo extends React.Component
{
    handleOptionsButtonClick(e){
        this.setState({showOptionsModal: true});
    }
    render(){
        return (
            <TouchableHighlight
                onPress={this.handleOptionsButtonClick.bind(this)} // bind绑定
                onPress={e=>this.handleOptionsButtonClick(e)} // 箭头函数
                >
                <Text>{this.props.label}</Text>
            </TouchableHighlight>
        )
    },
}
```

需要注意的是，不论是bind还是箭头函数，每次被执行都返回的是一个新的函数引用，
因此如果你还需要函数的引用去做一些别的事情（譬如卸载监听器），那么你必须自己保存这个引用

```js
class PauseMenu extends React.Component{
    constructor(props){
        super(props);
        this._onAppPaused = this.onAppPaused.bind(this);
    }
    componentWillMount(){
        AppStateIOS.addEventListener('change', this._onAppPaused);
    }
    componentDidUnmount(){
        AppStateIOS.removeEventListener('change', this._onAppPaused);
    }
    onAppPaused(event){
    }
}
```
