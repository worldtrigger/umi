# 路由

## 手动添加页面

- 在 src 目录下添加 Pageone,PageTwo,PageThree 三个文件夹

- 在三个文件夹下下新建对应的 tsx 文件

## 路由嵌套

- 找到 .umirc 里面写 routes

```javascript
import { defineConfig } from 'umi';

export default defineConfig({
  nodeModulesTransform: {
    type: 'none',
  },
  routes: [
    { path: '/', component: '@/pages/index' },
    {
      path: '/home',
      component: '@/pages/Home/Home',
      routes: [
        { path: '/home/page1', component: '@/components/Home/PageOne/PageOne' },
        { path: '/home/page2', component: '@/components/Home/PageTwo/PageTwo' },
        {
          path: '/home/page3',
          component: '@/components/Home/PageThree/PageThree',
        },
      ],
    },
  ],
  fastRefresh: {},
});
```

> 这样访问/home/page1,就会加载对应的组件。不用引入

## 重定向

- redirect

```bash

export default {
  routes: [
    { exact: true, path: '/', redirect: '/list' },
    { exact: true, path: '/list', component: 'list' },
  ],
}

```

## 路由权限验证

比如 可以用于路由级别的权限验证

```bash

export default {
  routes: [
    { path: '/user', component: 'user',
      wrappers: [
        '@/wrappers/auth',
      ],
    },
    { path: '/login', component: 'login' },
  ]
}

```

- 然后在 `src/wrappers/auth` 中

```bash

import { Redirect } from 'umi'

export default (props) => {
  const { isLogin } = useAuth();
  if (isLogin) {
    return <div>{ props.children }</div>;
  } else {
    return <Redirect to="/login" />;
  }
}

```

这样 访问 `/user` 通过 `useAuth` 做权限验证,如果通过了渲染 `src/pages/user`,否则则会跳转到 `/login`,由 `src/pages/login` 渲染

## 路由传递参数

### 路由跳转

- 路由跳转必须借助 history

```javascript
import * as React from 'react';
import { history } from 'umi';
interface IPageOneProps {}

const PageOne: React.FunctionComponent<IPageOneProps> = (props) => {
  function gotourl() {
    history.push({
      pathname: '/home/page2',
      state: {
        a: '传递的值',
      },
    });
  }
  return (
    <div>
      <span>第一页</span>
      <button onClick={gotourl}>跳转</button>
    </div>
  );
};

export default PageOne;
```

### 获取路由参数

- 跳转成功后,获取路由参数:props.location.state

```javascript
import * as React from 'react';

interface IPageTwoProps {}

const PageTwo: React.FunctionComponent<IPageTwoProps> = (props: any) => {
  console.log(props.location.state);
  if (props.location.state) {
    return (
      <div>
        <span>哈哈---{props.location.state.a}</span>
        <span>第二页</span>
      </div>
    );
  } else {
    return (
      <div>
        <span>第二页</span>
      </div>
    );
  }
};

export default PageTwo;
```
