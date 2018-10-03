---
title: React-native 笔记
date: 2017-07-12 16:47:59
tags: [ReactNative, 笔记]
keywords: [ReactNative,笔记]
categories: [技术笔记]
comments: false
---


# 前言

 - 对中文网的的 ReactNative 的内容进行的 `提炼`
 - 对其中的内容进行了 `简化`
 - 对一些我个人认为较复杂且不常用的功能或概念 `采取忽略`
 - 对 ReactNative `不了解` 的同学可能会有一点帮助
 - 如笔记整理有错误还请 [留言](https://github.com/jianxiaoBai/jianxiaobai.github.io/issues)
 - 深度阅读请访问 [ReactNative中文网](http://reactnative.cn/docs/0.46/getting-started.html#content)

 <!-- more -->

# ReactNative 使用

## 安装运行 ReactNative

 - `react-native init` AwesomeProject
 - cd AwesomeProject && `react-native run-ios`

<span id="inline-yellow">提示:</span> version参数 可用来指定版本,例如 `react-native init MyApp --version 0.44.3`。


-----------

## Hello World

```javascript
/* 引用 React 的默认方法 与 Component 组件 */
import React, { Component } from 'react';
/* 引用 react-native 的AppRegister 与 Text 组件 */
import { AppRegistry, Text } from 'react-native';
/* 定义 Hello 组件 并扩展到 react 的 Component 中*/
class Hello extends Component {
    /* JSX 渲染语法 */
  render() {
    return (
      <Text>Hello world!</Text>
    );
  }
}
/* AppRegistry的内置模块对 Hello组件 进行了“注册”操作,在整个应用里只会用一次 */
AppRegistry.registerComponent('Hello', () => Hello);

```

-----------

## Props(属性)

<p id="border-blue">多数组件在创建时就可使用 `props参数` 来定制。</p>

### Image组件使用 `props`

```javascript
import React, { Component } from 'react';
import { AppRegistry, Image } from 'react-native';

class Bananas extends Component {
  render() {
    let pic = {
      uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
    };
    return (
      <Image source={pic} style={{width: 193, height: 110}} />
    );
  }
}

AppRegistry.registerComponent('Bananas', () => Bananas);
```

### 自定义的组件使用 `props`

![](/imgs/RN-props.jpg)

-----------

## State(状态)

 - RN中可通过 props 和 state 来控制一个组件
   - `props` 数据是在父组件中指定，仅生效一次
   - `state` 数据可根据需求进行改写
 - `constructor` 可用来初始化state ,需要改写时调用 `setState` 方法

文字变换例子

![](/imgs/RN-state.jpg)

-----------

## Style(样式)

 - 按照JS的语法要求使用了驼峰命名法
 - `StyleSheet.create` 来集中定义组件的样式

```javascript
import React, { Component } from 'react';
import { AppRegistry, StyleSheet, Text, View } from 'react-native';

class LotsOfStyles extends Component {
  render() {
    return (
      <View>
        <Text style={styles.red}>just red</Text>
        <Text style={styles.bigblue}>just bigblue</Text>

        /* 常见的做法是按顺序使用属性，即后属性会覆盖前属性 */

        <Text style={[styles.bigblue, styles.red]}>bigblue, then red</Text>
        <Text style={[styles.red, styles.bigblue]}>red, then bigblue</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  bigblue: {
    color: 'blue',
    fontWeight: 'bold',
    fontSize: 30,
  },
  red: {
    color: 'red',
  },
});

AppRegistry.registerComponent('LotsOfStyles', () => LotsOfStyles);

```
-----------

## Flexbox 布局

<p id="border-yellow"> `flexDirection` 的默认值是 `column` 而不是row，而flex也只能指定一个数字值。</p>

 - Flexbox 主要三属性
   - flexDirection
   - justifyContent
   - alignItems

**居中排列**

```javascript
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class AlignItemsBasics extends Component {
  render() {
    return (
      // 尝试把`alignItems`改为`flex-start`看看
      // 尝试把`justifyContent`改为`flex-end`看看
      // 尝试把`flexDirection`改为`row`看看
      <View style={{
        flex: 1,
        flexDirection: 'row',
        justifyContent: 'center',
        alignItems: 'center',
      }}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
};

AppRegistry.registerComponent('AwesomeProject', () => AlignItemsBasics);
```

-----------

## TextInput

 - `TextInput` 是一个允许用户输入文本的基础组件。
 - `onChangeText` 属性接受一个函数，而此函数会在 `文本变化时` 被调用。
 - `onSubmitEditing` 属性，会在 `文本被提交后`（用户按下软键盘上的提交键）调用。

------

## ScrollView

 - ScrollView 是一个通用的可滚动的容器，其中可放入多个组件和视图，`不区分类型`,可以 `水平滚动`
 - ScrollView 适合用来显示 `数量不多` 的滚动元素

```javascript
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  ScrollView,
  Image
} from 'react-native';

export default class testApp extends Component {
  render() {
      return(
        <ScrollView>
          <Text style={{fontSize:96}}>Scroll me plz</Text>
          <Image source={require('./src/img/aaa.png')} style={{height:200,width:200}}/>
          <Image source={require('./src/img/aaa.png')} style={{height:200,width:200}}/>
          <Image source={require('./src/img/bbb.jpeg')} style={{height:200,width:200}}/>
          <Image source={require('./src/img/aaa.png')} style={{height:200,width:200}}/>
          <Image source={require('./src/img/aaa.png')} style={{height:200,width:200}}/>
          <Text style={{fontSize:96}}>If you like</Text>
          <Image source={require('./src/img/bbb.jpeg')} style={{height:200,width:200}}/>
          <Text style={{fontSize:80}}>React Native</Text>
        </ScrollView>
    );
  }
}
AppRegistry.registerComponent('testApp', () => testApp);
```
-----------

## ListView

 - RN 中展示长列表数据的常用组件有俩个
  - FlatList
  - SectionList

### FlatList 组件

 - 特点
   - 适用展示一组 `仅数据不同的垂直滚动列表`
   - 适用 `长列表数据` 且元素的个数可以增删
   - FlatList `优先渲染屏幕上可见的元素`

 - 使用
   - FlatList 组件 `必须` 的两个属性是 `data` 和 `renderItem` 。
     - data 是列表的 `数据源`
     - renderItem 是从数据源中逐个 `解析数据` ，然后返回一个设定好格式的组件来渲染。

```javascript

import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  ScrollView,
  Image,
  FlatList
} from 'react-native';

export default class testApp extends Component {
  render() {
    return (
      <View style={styles.container}>
        <FlatList
          data={[
            {key: 'Devin'},
            {key: 'Jackson'},
            {key: 'James'},
            {key: 'Joel'},
            {key: 'John'},
            {key: 'Jillian'},
            {key: 'Jimmy'},
            {key: 'Julie'},
            {key: 'Julie'},
            {key: '白晓健'},
            {key: '白晓建'},
            {key: '白晓见'},
            {key: '白小剑'},
          ]}
          // renderItem 一个对象, 里面有一个箭头函数其参数是一个对象,对象的值时 data 中的 每一项
          renderItem={({item}) => {

            return <Text style={styles.item}> 你好{item.key} ,welcome </Text>
      }}
        />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
   flex: 1,
   paddingTop: 22,
  },
  item: {
    padding: 10,
    fontSize: 18,
    height: 44,
    backgroundColor:'pink'
  },
})
AppRegistry.registerComponent('testApp', () => testApp);
```

### SectionList

<p id="border-blue">如果想提供一组数据分成 `逻辑部分` 或 `章节标题` ，可以使用 `SectionList` 。</p>


## 网络请求(fetch)

### 发起网络请求

<p id="border-blue">想从地址获取内容,只需将网址作为参数即可</p>

```javascript

export default class testApp extends Component {
  state = {
    movies:[
      {key: '白晓健'},
      {key: '白晓建'},
      {key: '白晓见'},
      {key: '白小剑'},
    ],
  };

  fatchData=()=>{
    fetch('https://api.douban.com/v2/movie/in_theaters')
    // 转换成 Text  是为了当意外发生时,更容易锁定错误
    .then((response) => response.text())
    .then((responseText) => {
      const json = JSON.parse(responseText);
      console.log(json)
      // 重新设置 state 中 movies 的 值,也就是给 movies 赋值
      this.setState({movies:json.subjects});
    })
    .catch((error) => {
      console.error(error);
    });
  }
  componentDidMount() {
      this.fatchData()
  }

  render() {
    return (
      // const { movies } = this.state;
      <View style={styles.container}>
        <FlatList
          data={this.state.movies}
          // 一个对象 里面有一个
          renderItem={({item}) => {

            return <Text style={styles.item}> 你好,{item.title} ,welcome </Text>
      }}
        />
      </View>
    );
  }
}

```

fetch 的第二个可选参数,可用来定制HTTP请求一些参数

```javascript
/* 指定header参数，指定使用POST方法，提交数据 */
fetch('https://mywebsite.com/endpoint/', {
  method: 'POST',
  headers: {
    'Accept': 'application/json',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    firstParam: 'yourValue',
    secondParam: 'yourOtherValue',
  })
})

```

### 处理响应数据

 - 网络请求天生就是一种异步操作
 - Fetch 方法会返回一个 `Promise实例` ，其用意是简化异步风格的代码
 - 默认情况下，iOS会 `阻止所有非https的请求` ,[解决方案](https://segmentfault.com/a/1190000002933776)

```javascript
getMoviesFromApiAsync() {
   return fetch('https://facebook.github.io/react-native/movies.json')
     .then((response) => response.json())
     .then((responseJson) => {
       return responseJson.movies;
     })
     .catch((error) => {
       console.error(error);
     });
 }
```

RN 应用中使用 `ES7标准中` 的async/await 语法：

```javascript
// 注意这个方法前面有async关键字
 async getMoviesFromApi() {
   try {
     // 注意这里的await语句，其所在的函数必须有async关键字声明
     let response = await fetch('https://facebook.github.io/react-native/movies.json');
     let responseJson = await response.json();
     return responseJson.movies;
   } catch(error) {
     /* 别忘了catch住fetch可能抛出的异常，否则出错时你可能看不到任何提示。 */
     console.error(error);
   }
 }

```

---------

### 使用其他的网络库

 - RN 中已经内置了 `XMLHttpRequest` 也就是 ajax
 - 基于 `XMLHttpRequest` 封装的第三方库有 `frisbee` 或是 `axios` 等
 - 没有跨域限制

### WebSocket

<p id="border-blue">RN 支持 WebSocket，这种协议可以在单个 `TCP` 连接上提供 `全双工` 的通信信道</p>

[深度阅读](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

--------

## 使用导航器跳转页面

<p id="border-yellow">从0.44版本开始， `Navigator` 被从react native的核心组件库中 `抽离` 到了一个名为 `react-native-deprecated-custom-components` 的单独模块中。
如果需要继续使用 `Navigator` ，则需要先 `yarn add react-native-deprecated-custom-components` 安装，然后从这个模块中 import，即 `import { Navigator } from 'react-native-deprecated-custom-components'` .</p>


### React Navigation

<p id="border-blue">社区今后主推的方案是一个 `单独` 的导航库 `react-navigation` ，它的使用十分简单。</p>

[详细了解](https://reactnavigation.org/docs/intro/)

首先是在当前应用中安装此库

```javascript
yarn add react-navigation
```

基本例子

```javascript
import React from 'react';
import {
  AppRegistry,
  Text,
  View,
  Button
} from 'react-native';
import { StackNavigator } from 'react-navigation';

/* 导入组件 */
import Detail from './src/components/Detail';
import ChatScreen from './src/components/Chat';
import TestScreen from './src/components/Test';

class HomeScreen extends React.Component {

  /* 定义本页面的导航栏设置*/
  static navigationOptions = {
    /* 定义标题,此标题也是跳入其他页面时返回键上的信息 */
    title: 'Welcome',
  };

  render() {
    /* 使标题生效 */
    const { navigate } = this.props.navigation;

    return (
      <View>
        <Text>Hello, Chat App!</Text>
        <Button
          /* 设置文本点击事件,进入指定组件 */
          onPress={() => navigate('Chat')}
          title="Chat with Lucy"
        />
      </View>
    );

  }
}

/* 在根文件中用 StackNavigator 声明这些组件(类似于定义路由),键就是 路由名称  */
const testApp = StackNavigator({
  Home: { screen: HomeScreen },
  aaa: { screen: Detail },
   Chat: { screen: ChatScreen },
   Test: { screen: TestScreen },
});

AppRegistry.registerComponent('testApp', () => testApp);
```


## 图片

### 静态图片资源引用方式

```javascript
// 正确
<Image source={require('./my-icon.png')} />

// 错误
var icon = this.props.active ? 'my-icon-active' : 'my-icon-inactive';
<Image source={require('./' + icon + '.png')} />

// 正确
var icon = this.props.active ? require('./my-icon-active.png') : require('./my-icon-inactive.png');
<Image source={icon} />

```

注意:require中的图片名字必须是一个静态字符串, `不能使用变量` ！


### 静态非图片资源

<p id="border-blue">`require` 语法也可以用来静态地加载你项目中的声音、视频或者文档等文件。</p>

注意: `视频必须指定尺寸` 而不能使用flex样式.

### 网络图片

```javascript
// 正确
<Image source={{uri: 'https://facebook.github.io/react/img/logo_og.png'}}
       style={{width: 400, height: 400}} />

// 错误
<Image source={{uri: 'https://facebook.github.io/react/img/logo_og.png'}} />
```

可携带参数

```javascript
<Image source={{
  uri: 'https://facebook.github.io/react/img/logo_og.png',
  method: 'POST',
  headers: {
    Pragma: 'no-cache'
  },
  body: 'Your Body goes here'
}}
style={{width: 400, height: 400}} />
```

### 背景图片组件

开发者们常面对的一种需求就是类似web中的背景图（background-image）。要实现这一用例，只需简单使用<ImageBackground>组件，然后把需要背景图的子组件嵌入其中即可。

```javascript
return (
  <ImageBackground source={...}>
    <Text>Inside</Text>
  </ImageBackground>
);
```


## 处理触摸事件

### 点击事件

```javascript
class MyButton extends Component {
  _onPressButton() {
    console.log("You tapped the button!");
  }

  render() {
    return (
      <TouchableHighlight onPress={this._onPressButton}>
        <Text>Button</Text>
      </TouchableHighlight>
    );
  }
}
```

 - 具体使用哪种组件，取决于希望给用户什么样的视觉反馈：
  - `TouchableHighlight`:来制作按钮或者链接。用户手指按下时 `背景变暗`
  - `TouchableNativeFeedback`:用户手指按下时形成类似 `墨水涟漪` 的视觉效果(限Android)
  - `TouchableOpacity`:会在用户手指按下时 `降低透明度`
  - `TouchableWithoutFeedback`:点击事件的时 `无效果`

<p id="border-blue">某些场景中你可能需要检测用户是否进行了 `长按操作` 。
可以在上面列出的任意组件中使用 `onLongPress` 属性来实现。</p>

### 列表滑动

 - ScrollView
  - 可实现用户会在列表中或快或慢的 `各种滑动`
  - 还可以配置 `pagingEnabled` 属性来让用户 `整屏滑动`
  - 水平方向的滑动还可以使用Android上的 `ViewPagerAndroid` 组件。
 - ListView
  - 是一种特殊的ScrollView，用于显示 `较长垂直列表`

---------

## 动画

 - RN 提供了两个互补的动画系统：
   - 用于 `全局` 的布局动画 `LayoutAnimation`
   - 用于创建更 `精细` 的交互控制的动画 `Animated`

[深度阅读](http://reactnative.cn/docs/0.46/animations.html#content)

-------

## 定时器

RN 实现了和浏览器一致的定时器 Timer 。

 - 定时器
  - setTimeout, clearTimeout
  - setInterval, clearInterval
  - `setImmediate`, clearImmediate
  - `requestAnimationFrame`, cancelAnimationFrame

setImmediate 和 setTimeout 有略微不同.
requestAnimationFrame 和 setInterval 有略微不同.

[详细阅读](http://reactnative.cn/docs/0.46/timers.html#content)

注意: `卸载组件前务必清除定时器`


-------


## 直接操作

<p id="border-blue">在 RN 中， `setNativeProps` 就是等价于直接在底层操作DOM节点的方法。</p>

注意: 在使用 `setNativeProps` 之前,先尝试用 `setState` 或 `shouldComponentUpdate` 方法来解决问题

-----------

# ReactNative 问题

## 端口占用

<p id="border-red">当运行时提示 `“Packager can’t listen on port 8081”` ，说明 8081 端口被占用</p>

 - 第一种
   - 检查占用端口的程序并关闭
 - 第二种
   1. 启动服务时指定端口号 `react-native start --port 8083`
   2. 手动修改项目下的 `node_modules/react-native/local-cli/server/server.js` 中的 port 字段
