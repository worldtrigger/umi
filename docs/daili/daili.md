# 请求代理和部署

> 有的时候我们需要跨域访问

## 配置 proxy

在根目录下有个 umirc.ts 文件里面加入

```javascript
export default {
  "proxy": {
    "/api": {                                       ---step1
      "target": "https://pvp.qq.com", ---step2
      "changeOrigin": true,                         ---step3
      "pathRewrite": { "^/api" : "" }               ---step4
    }
  }
}
```

## 使用

这样访问的时候就可以不用 target 里面的地址直接使用 api 即可

比如要访问`https://pvp.qq.com/student`

可以写成

```javascript
/api/student
```

## 部署

当你部署在非根目录的时候必须用 base 解决

比如服务端给你的访问链接是 https://www.xxx.com/web

base 里面就应该这样写

```javascript
export default {
  base: '/web',
};
```
