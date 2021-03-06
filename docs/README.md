# 快速开始

## 什么是 umi

umi 是由 dva 的开发者云谦编写的一个新的 React 框架,umi 既是一个框架也是一个工具,你可以将它简单的理解为一个专注性能的类 next.js 前端框架,并通过约定，自动生成和解析代码等方式来辅助开发,减少开发者的代码量.

umi 是通用方案,适用于现在几乎所有的 web 环境

## umi 的优势

umi 是一个专注性能的类 next.js 端框架,它的优势是:

- 内置大量的性能优化

- 多端无缝支持容器和浏览器访问

- 类 webpack 的插件机制

- 针对 antd 和 dva 有友好的支持

umi 最显著的特点就是[文件即路由] -- 在 pages 文件夹下新建文件,umi 将自动生成与文件路径对应的路由。在大部分其他前端框架中，路由配置一直是一个很麻烦的事情，而对于多人协作开发的项目,公共的配置文件则可能面临更多的冲突

## umi 的可扩展性

作者称 `umi有着类webpack般灵活的插件机制,他就是一个架子`.主要的 umi 项目不到 700 行代码,umi 负责搭建好骨架。把框架的生命周期钩子暴露出来,然后通过插件来丰富功能

你可以用高达玩具类比 umi 的可扩展性:刚入手的玩家可以依据说明书,一步步的组装出自己心爱的玩具。对于高玩来说,官方提供了一个骨架.保证了高达的可动性,然后你自己可以随意 DIY，任意的使用材料和设计方式

刚接触前端的同学可以很好的完成公司的业务需求,对前端有一定了解的同学可以随意的修改,包括配置,编译,开发,模板，请求方式,数据流等等。几乎所有能想到的前端工程化的内容,都允许自定义。在一步步接触这些可配置项的时候,你也会一步步的对前端工程化有更多的认识和理解

## umi 的性能

在项目性能方面 umi 已经帮你做了很多优化,包括构建产物的大小,执行效率,首屏加载,用户体验等方面,但这些优化对于开发者是无感的,有时候你升级了一下插件版本,整个项目可能就跟着优化了，而不需要你进行其他调整。作者称:`你只管写业务代码,我会负责性能,并且随着umi的迭代,我保证你的应用会越来越快`

简单来说:umi 做到了开箱即用,对于开发者和前端初学者都是非常友好的

## Why umi

- 有时候我希望我的项目可以跑在支付宝(淘宝)容器里(多页),又可以泡在普通浏览器(单页)

- 随着项目越来越大,开发调试的启动和热更新时间越来越长

- 我要部署在静态服务器或者 cdn 上,能否帮我把 HTML 也生成出来

- ts,jest,babel 的配置好麻烦,而且配了这个和另外那个犯冲突，怎么解决
