# 五分钟掌握最小知识体系

本文阅读时间大概 5 分钟,但是让你了解基于 umi 和 dva 构建项目的最小知识体系,这样保证效率

## ECMAScript6

### 变量声明

const 用于声明常量,let 用于声明变量,他们都是块级作用域

```javascript
const a = 1;
let b = 1;
```

### 模板字符串

用于拼接字符串

```javascript
let a = 'hello';
let b = 'hello';
console.log('print:' + a + b);
let c = `print:${a}`;
```

### 默认参数

```javascript
function test(a = 'world') {
  console.log(`print:hello,${a}`);
}
test();
```

### 箭头函数

```javascript
function test(a = 'world') {
  console.log(`print:hello,${a}`);
}

const test = (a = 'world') => {
  console.log(`print:hello,${a}`);
};
```

### 模块的导入和导出

```javascript
// 从antd中导入按钮

import { Button } from 'antd';

//导出一个方法,这样就能在其他文件使用import导入使用了

const test = (a = 'world') => {
  console.log(`print:hello,${a}`);
};
```

### 解构赋值

```javascript
const obj = { key: 'umi', author: 'sorrycc' };
console.log(obj.key);

const { key } = obj;

const arr = [1, 2];
const [foo, bar] = arr;
console.log(foo);
```

### 展开运算符

- 用于数组组装

```javascript
const arr = ['umi'];
const texts = [...arr, 'dva'];
//texts = ['umi','dva']
```

- 用于取出数组部分属性

```javascript
const arr = ['umi', 'dva', 'antd'];
const ['umi', ...other] = arr;
//前面已经提过解构赋值,所以第一项会赋值给`umi`,剩下的会被组合成一个`other`数组
console.log(umi);
//umi
console.log(other);
//['dva','antd']
```

- 用于组合新的对象,key 相同时,靠后展开的值会覆盖靠前的值

```javascript
const obj = { a: 1, b: 2 };

const obj2 = { b: 3, c: 4 };

const obj3 = { ...obj, ...obj2 };
```

## JSX

### 组件嵌套

类似 HTML

```javascript
<app>
  <Header />
  <Footer />
</app>
```

### className

class 是 Javascript 的保留词,所以添加样式类名的时候,需用 className 代替 class

```javascript
<h1 className="demo1">Hello Umi</h1>
```

### Javascript 表达式

- Javascript 用{}括起来,会执行返回的结果

```javascript
<h1>{this.props.title}</h1>
```

- 注释 尽量不用//做单行注释

```javascript
<h1>{/*  */}</h1>
```

## 理解 CSS Modules

```javascript
import styles from './example.css';

const Example = <button className={styles.button}>Click Me </button>;

/*
 *
 * .button{
 *  background:#1890ff;
 * }
 *
 *
 */
```

你不必理解 CSS Modules 的工作原理,只需要 import from 必须写在文件的开头.你可以为样式类名起一个简短的描述性名字,而不用关心命名冲突的问题

## Dva

- Model

在 umi 项目中,你可以使用 dva 来处理数据流,以响应一些复杂的交互操作。这些处理数据流的文件统一放在 models 文件夹下,每一个文件默认导出一个对象,里面包含数据和处理数据的方法,我们通常称之为 model,结构如下

```javascript
export default {
  namespace:'example', //Model的名字,必须全局唯一
  state:{
    count:0,
  }, //初始数据
  reducers:{
    save(){...},
  }, //用于修改数据
  effects:{
    *getData(){...},  //用于获取数据
  },
  subscriptions:{
    setup(){...}  //用于订阅数据
  }
}
```

- Reducer

每一个 Reducer 都是一个普通的函数,接受 state 和 action 作为参数即:(state,action)=>state,你可以在函数中更改旧的 state,返回新的 state。

理论上应该传递 action 但实际上把 action 解构就变成{type,payload},所以直接写 payload 即可

```javascript

reducers:{
  save(state,{payload}){}
}

```

- Effect

每一个 Effect 都是一个生成器函数,也叫做\*号函数,你可以在这里获取你需要的数据,例如向服务器发送一个请求，或者获取其他 model 里的 state.为了明确分工,你无法在 effect 中直接修改 state,但你可以通过 put 方法调用 reducer 来修改 state

```javascript
state:{
  assets:{}
}
*changeassets({payload},{call,put,select}){
   const user = yield select(states=>states.user) // 用于获取其他模块
   const assets = yield call(fetchData,user) //异步获取数据,第二个参数就是发送的数据
   yield put({type:'save',payload:{assets}}) //推向state
}
```

`select`

此方法用于获取当前或者其他 model 的 state

```javascript

const data = yield select(states=>states.user)

```

`call`

此方法用于执行一个异步函数,可以理解为等待这个函数结束。项目中常用于发送 http 请求,等待服务端响应数据。

```javascript
const data = yield call(异步方法,给异步方法的参数)
```

`put`

此方法用于触发 action ,这个 action 既可以是一个 reducer,也可以是一个 effect

```javascript
yield put({type:'reducerName',payload:要传的值})
```

- Subscription

最主要的就是监听，这里是自动发送,发送到 reducers

```javascript

  subscriptions: {
    setup({ history, dispatch }) {
      return history.listen((location, action) => {
        console.log(location.pathname);
        console.log(location.state);
        console.log(action);
         //history.push()
        if (location.pathname === "/about") {
          dispatch({
            type: "changevalue",
            payload: "测试的公共数据",
          });
        }
      });
    },
  },

```

`dispatch`

类似 effect 中的 put 方法,你可以在 subscription 的参数,或者是一个已经 connect 过的组件的 props 中拿到

`connect`

此方法在你的组件中获取指定 model 的 state 数据

```javascript
import React, { useState, useEffect } from 'react';
import List from '../components/Product/ProductList.js';
import { connect } from 'dva';
import { withRouter } from 'dva/router';
const ProductExample = (props) => {
  useState(() => {
    props.handlegetList();
    props.handlegetResultId();
  }, []);
  function gotourl() {
    console.log(props);
    props.history.push({
      pathname: '/',
      state: {
        id: 3,
      },
    });
  }
  function getId() {
    if (props.history.location.state) {
      return <div>{props.history.location.state.id}</div>;
    } else {
      return null;
    }
  }
  if (props.list.length > 0) {
    console.log(props.list);
    return (
      <div>
        <p>这就是测试页面</p>
        <List data={props.value}></List>
        <ul>
          {props.list.map((content, index) => {
            return (
              <li key={index}>
                {content.id}---{content.value}
              </li>
            );
          })}
        </ul>
        <div>
          {props.resultdata.id}---{props.resultdata.value}
        </div>
        <button onClick={gotourl}>点击跳转</button>
        {getId()}
      </div>
    );
  } else {
    return null;
  }
};

const mapState = (state) => {
  return {
    value: state.product.value,
    list: state.product.list,
    resultdata: state.product.resultdata,
  };
};

const mapMutations = (dispatch) => {
  return {
    handlegetList() {
      dispatch({
        type: 'product/getvalue',
      });
    },
    handlegetResultId() {
      dispatch({
        type: 'product/getId',
      });
    },
  };
};

export default connect(mapState, mapMutations)(withRouter(ProductExample));
```
