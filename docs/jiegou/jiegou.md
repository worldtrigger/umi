# 项目结构

- 项目结构

```Bash

├── mock
├── package.json
├── src
│   ├── app.js
│   ├── assets
│   │   └── yay.jpg
│   ├── global.css
│   ├── layouts
│   │   ├── index.css
│   │   └── index.js
│   ├── models
│   └── pages
│       ├── index.css
│       └── index.js
├── .env
└── .umirc.js

```

这就是我们当前的目录结构,使用 create-umi 创建的项目,是一个典型的 umi 项目结构,我们可以在 umi 约定的使用目录中,添加需要的内容

## mock

约定 mock 目录里所有.js 文件会被解析为 mock 文件。比如新建 mock/user.js 内容如下:

```javascript
export default {
  '/api/users': ['a', 'b'],
};
```

然后在浏览器里访问 `http://localhost:8000/api/users` 就可以看到 ['a', 'b'] 了

## src

我们约定将项目的所有源码放在 src 目录,umi 目录的大部分的工作都在这里进行

当我们运行 `umi dev` 或者 `umi build` 时,此目录下的代码会被转换成浏览器能够运行的正确的 Javascript 版本,所以我们可以在这里使用 TS 或者其他的 JS 语法

## src/layouts/index.js

- 约定式的全局布局,实际上是在路由外面套了一层。比如:你的路由

```javascript
[
  {
    path: '/',
    component: './pages/index',
  },
  {
    path: '/users',
    component: './pages/users',
  },
];
```

- 如果有 layouts/index.js 那么路由则变为:

```javascript
[
  {
    path: '/',
    component: './layouts/index',
    routes: [
      {
        path: '/',
        component: './pages/index',
      },
      {
        path: '/users',
        component: './pages/users',
      },
    ],
  },
];
```

- 组件角度可以简单的理解

```bash

<layout>
	<page>1</page>
	<page>2</page>
</layout>

```

> 只要存在 layouts/index.js 就生效,如果你不需要这个,那么只能将它重命名为别的名字

## src/pages

约定 pages 下所有的(j|t)sx?文件即路由,在 umi 中可以使用约定式路由和配置式路由,在实际开发中,我个人更偏向于使用,约定式路由.毕竟是 umi 的主要特性之一,使用约定式路由,意味着不需要维护可怕的路由配置文件。最常用的有基础路由和动态路由(用于详情页,需要从 url 取参数的情况)

### 基础路由

假设 pages 目录结构如下:

```javascript
├── pages
│   ├── users
│   │   └── index.js
│   │   ├── list.js
├── index.js

```

umi 将自动生成如下的路由配置

```javascript
[
  {
    path: '/',
    component: './pages/index.js',
  },
  {
    path: '/users/',
    component: './pages/users/index.js',
  },
  {
    path: '/users/list',
    component: './pages/users/list.js',
  },
];
```

### 动态路由

umi 约定被[]包裹的目录或文件为动态路由,比如如下结构

```bash
├── pages
│   ├── [post]/
│   │   └── index.js
│   │   ├── comments.js
│   ├── users/
│         ├── [id].js
│         ├── index.js
```

会生成路由配置如下；

```javascript
[
  {
    path: '/',
    component: './pages/index.js',
  },
  {
    path: '/users/:id',
    component: './pages/users/$id.js',
  },
  {
    path: '/:post/',
    component: './pages/$post/index.js',
  },
  {
    path: '/:post/comments',
    component: './pages/$post/comments.js',
  },
];
```

### src/pages/404.js

当访问的路由地址不存在的时候,会显示 404 页面,只有 build 后生效,调试的时候可以访问 `/404`

> 注意开发模式下会使用内置 umi 提供的 404 提示页面

### src/.umi

这是 umi dev 时生产的临时目录,默认包含 umi.js 和 router.js 有些插件也会在这里生成一些其他临时文件。

`但是请不要在这里修改代码`

### .umirc.ts 和 config/config.js

umi 的配置文件 二选一。当两者同时存在,优先使用.umirc.ts
