# 使用 CLI 创建初始化项目

## 使用 yarn 初始化项目

```bash

yarn global add @umijs/create-umi-app

```

## 使用 create-umi-app 新建项目

```bash

$ create-umi-app
Copy:  .editorconfig
Write: .gitignore
Copy:  .prettierignore
Copy:  .prettierrc
Write: .umirc.ts
Copy:  mock/.gitkeep
Write: package.json
Copy:  README.md
Copy:  src/pages/index.less
Copy:  src/pages/index.tsx
Copy:  tsconfig.json
Copy:  typings.d.ts

```

如果你的命令行打印的日志如上,说明新的项目完成了,如果有错误,确认下当前目录是否为空

## 安装依赖

```bash

$ yarn
...这个过程需要一些时间
success Saved lockfile.
✨  Done in 170.43s.

```

## 启动开发服务器

```javascript

$ yarn start
$ umi dev
Starting the development server...

✔ Webpack
  Compiled successfully in 7.21s

 DONE  Compiled successfully in 7216ms                                  14:51:34


  App running at:
  - Local:   http://localhost:8000 (copied to clipboard)
  - Network: http://192.168.10.6:8000

```

## 访问

```javascript

httP://localhost:80000

```
