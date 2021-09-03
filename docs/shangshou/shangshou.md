# 开发环境搭建

## 安装 Node

- 去官方网站下载 node.js，然后安装

```javascript
node -v

V8.10.1
```

## 安装 vscode

- 下载 vscode

- 安装 Oni 插件

这样以后输入 umi 就能有提示

## 快速上手 Demo

### 安装 umi

```bash

$ cnpm install -g umi

```

### 新建一个简单的 umi 项目

```javascript

umi g page home

```

- 这样你就会得到如下的目录

```bash

└── pages
    ├── home.css
    ├── home.js
    └── list.js

```

### 启动开发服务器

```bash

$ umi dev

  App running at:
  - Local:   http://localhost:8000/ (copied to clipboard)
  - Network: http://192.168.199.195:8000/

```

### 访问的时候

```javascript

http://localhost:8000/home

```
