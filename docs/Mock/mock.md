## Mock 数据

> Mock 数据式前端开发过程中必不可少的一环，是前后端分离的关键链路。通过请求数据甚至逻辑，能够让前端开发独立自主，不会被服务端的开发阻塞

### 约定式 Mock 文件

- umi 约定 `/mock` 文件夹下所有文件为 mock 文件

例如

```javascript

├── mock
    ├── api.ts
    └── users.ts
└── src
    └── pages
        └── index.tsx

```

`/mock` 下的 api.ts 和 users.ts 都会被解析为 mock 文件,只看接口不看文件

### 编写 Mock 文件

如果`/mock/api.ts` 的内容如下

```javascript
export default {
  // 支持值为 Object 和 Array
  'GET /api/users': { users: [1, 2] },

  // GET 可忽略
  '/api/users/1': { id: 1 },

  // 支持自定义函数，API 参考 express@4
  'POST /api/users/create': (req, res) => {
    // 添加跨域请求头
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.end('ok');
  },
};
```

### 支持引入 Mock.js

- 安装文件

```bash

yarn add mockjs -D

```

- 模拟数据

```javascript
import mockjs from 'mockjs';
export default {
  'GET /api/student': mockjs.mock({
    'list|100': [
      {
        name: '@city',
        'price|1-100': 1,
        'type|1-3': 2,
      },
    ],
  }),
};
```

### 当你需要使用插件模拟延迟

- 安装包依赖

```javascript

yarn add roadhog-api-doc -D

```

- 使用

```javascript
import mockjs from 'mockjs';
import { delay } from 'roadhog-api-doc';
const mockdata = {
  'GET /api/student': mockjs.mock({
    'list|100': [
      {
        name: '@city',
        'price|1-100': 1,
        'type|1-3': 2,
      },
    ],
  }),
};

export default delay(mockdata, 5000);
```
