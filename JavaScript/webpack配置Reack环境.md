---
title: webpack配置Reack环境
date: '2018-06-03'
tags: 
  - wabpack
  - react
  - babel
---


**感想：不想当门外汗，就直接看一手资料，少去看别人教程。**

网上的好多教程都有些过时了，要么是“年代久远”（现在都 2018 年了，而过期教程都是 15,16），要么就是时效过了（webpack 4 都发布好久了结果升级指南普遍是刚发布时的资讯状态）。而且不同教程的配置通常存在不同之处，有时就难以理解和分辨。还是官网的信息发布及时准确，比起去折腾“多样化”的配置方案，遵照官方的建议实在省力多了。我不建议你直接使用本仓库配置，因为这只是我的个人 webpack 模板，生搬硬套不一定适合你。

之前依葫芦画瓢搞过 Webpack 的 React 配置。再配置就忘得差不多了。于是建了这个仓库，做一个 Webpack + React 的示例，以做备忘。

# 需要的包

+ babel-core
    <br /> babel 核心库
+ babel-loader
+ babel-preset-env
    <br /> 读取`.babelrc`中的配置信息，使得 babel 根据你的配置进行 compile
    <br /> @ref <https://github.com/babel/babel/tree/master/packages/babel-preset-env>
    <br /> @ref <https://babeljs.io/docs/plugins/preset-env>
+ babel-preset-react
    <br /> compile JSX 语法
    <br /> @ref <https://babeljs.io/>
+ css-loader
+ extract-text-webpack-plugin
    <br /> 从 bundle 文件中分离出单独的文件
    <br /> @ref <https://webpack.js.org/plugins/extract-text-webpack-plugin/#src/components/Sidebar/Sidebar.jsx>
+ file-loader
    <br /> 令 webpack 返回指定对象的公开 URL
    <br /> @ref <https://webpack.js.org/loaders/file-loader/>
+ html-loader
    <br /> @ref <https://webpack.js.org/loaders/html-loader/>
+ html-webpack-plugin
    <br /> 新建和你生成的 *.bundle.js 搭配的 HTML 文件。
+ mini-css-extract-plugin
+ style-loader
    <br /> @ref <https://webpack.js.org/loaders/style-loader/>
+ webpack
+ webpack-cli
+ webpack-dev-server
+ webpack-merge


# .babelrc

一开始我也一脸蒙蔽(bi),`.babelrc`文件是干啥的？有的教程只是告诉你这样写就对了，不知其所以然。但是 Babel 的[官网](https://babeljs.io/docs/usage/babelrc/)上这个问题实际上将得很清楚。

我的理解是`.babelrc`实际上就是和一个配置文件，性质和作用和`webpack.config.js`差不多。不过文件是被 Babel 读取，解释。而且初学者只会接触到这样的配置：

```json
{
  "presets": ["env", "react"]
}
```

这段配置非常简短，也就导致更不易理解。

一步一步解释。

<!--我猜想 -->Babel 在读取这个文件后会调用`parsing JSON`函数，将其转化为一个 JS 对象。`{ ... }`这对大括号以及其里面格式就不难理解。这个文件本质上应该是一个 JSON 文件。你可以删掉一个花括号，看一下报错。

那里面的`"presets"`又是什么意思？

这个相当于声明你要使用的 *preset* ，比如上面的 `env`，`react` 就分别指之前安装的 `babel-preset-env`， `babel-preset-react`。

emmm， **preset**又是什么东东？

这是在官网找到的解释。

>Babel preset that automatically determines the Babel plugins you need based on your supported environments.

意思差不多就是：预先选择在你支持的运行环境上所需要的 Babel 编译插件。


比如：

```json
{
    "presets": [
        [ "env", {
                "targets": {
                "chrome": 52
            }
        } ]
    ]
}
```

这表示 Babel 最终编译出来的文件要支持在 Chrome 52 上运行。

而我们只有`"env"`就是使用默认设置，**把 ES2015+ 的语法 complie 成 ES2015 以前的**。

`'react'`就是指让 Babel complie JSX 的语法。去掉这个配置的话凡是有 JSX 语法的地方都会报错。

<!-- 
# Webpack config

> > > To Be Continue -->
