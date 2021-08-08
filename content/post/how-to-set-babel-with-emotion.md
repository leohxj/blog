---
title: "How to Set Babel With Emotion"
date: 2021-08-08T10:50:57+08:00
tags: ["React", "Emotion", "css-in-js"]
categories: ["前端技术"]
---

emotion 是一个 css-in-js 的技术方案, 它提供的 `css props` 是我最喜欢的功能.

## 结合 TypeScript 的设置

2021 年了, 项目肯定得上 TS 了, 最头疼的就是类型提示了, 由于 `css props` 是在标签上添加了自定义属性完成的, 所以我们得让 tsc 知道 jsx 来自 `@emotion/react`, 修改 `tsconfig.json`:

```json
{
  "compilerOptions": {
    "jsxImportSource": "@emotion/react"
  }
}
```

tsconfig 中还有一项控制 jsx 转换的: [TypeScript: TSConfig Reference - Docs on every TSConfig option](https://www.typescriptlang.org/tsconfig#jsx).

如果工程使用 babel 处理, 是不会使用这个配置 babel 转换有自己的 options: [@babel/preset-typescript · Babel](https://babeljs.io/docs/en/babel-preset-typescript#options).

BUT, 在 Parvel V2 构建的时候, 这里设置的 `jsxImportSource` 会影响最终的输出, [parcel-bundler/parcel - README.md](https://sourcegraph.com/github.com/parcel-bundler/parcel/-/blob/packages/transformers/babel/README.md), 会读取 tsconfig 的配置:
[JSTransformer.js - parcel-bundler/parcel - Sourcegraph](https://sourcegraph.com/github.com/parcel-bundler/parcel/-/blob/packages/transformers/js/src/JSTransformer.js).

但如果是用 tsc 去编译的话, 那么需要加上 [`JSX Pragma`](#jsx-pragma) 注释;

## 为什么需要设置 babel

主要是为了使用 `css props` 特性, 让编译出来的代码,使用 emotion 提供的 `jsx` 来处理转换.

但不添加额外配置, 其实也是能工作的 , 利用 [`JSX Pragma`](#jsx-pragma) , 需要我们为每个文件添加注释:

```tsx
// tell babel to convert jsx to calls to a function called jsx instead of React.createElement
/** @jsx jsx */
import React from "react";
```

ps: 这种情况下, 是使用的 classic JSX Transform, 如果我们想要更加优雅的自动使用 Emotion 的 jsx , 需要添加 `@emotion/babel-preset-css-prop`.

### 什么是 JSX Transform

> Browsers don’t understand JSX out of the box, so most React users rely on a compiler like Babel or TypeScript to transform JSX code into regular JavaScript. Many preconfigured toolkits like Create React App or Next.js also include a JSX transform under the hood.

### Classic JSX Transform

看个示例:

```tsx
function App() {
  return <h1>Hello World</h1>;
}

//  After Classic JSX Transform
function App() {
  return React.createElement("h1", null, "Hello world");
}
```

### New JSX Transform

React 17 开始使用 New JSX transform:

- [Introducing the New JSX Transform – React Blog](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)
- [7.9.0 Released: Smaller preset-env output, Typescript 3.8 support and a new JSX transform · Babel](https://babeljs.io/blog/2020/03/16/7.9.0#a-new-jsx-transform-11154-https-githubcom-babel-babel-pull-11154)

看个示例:

```tsx
function App() {
  return <h1>Hello World</h1>;
}

//  转换为
function App() {
  return jsx("h1", null, "Hello world");
}
```

可以看到, 这时候转换出来的代码, 就不强依赖 React 模块了, 而是一个 `jsx` 函数.

#### 开启方式

为了保持兼容性, 是需要我们自己开启的, 开启的好处是:

- With the new transform, you can use JSX without importing React.
- Depending on your setup, its compiled output may slightly improve the bundle size.
- It will enable future improvements that reduce the number of concepts you need to learn React.

开启方式:

**`@babel/preset-react`**:

```json
{
  "presets": [
    [
      "@babel/preset-react",
      {
        "runtime": "automatic"
      }
    ]
  ]
}
```

**`@babel/plugin-transform-react-jsx`**:

```json
{
  "plugins": [
    [
      "@babel/plugin-transform-react-jsx",
      {
        "runtime": "automatic"
      }
    ]
  ]
}
```

## 依据场景使用不同 Babel 方案

目的都是使用 emotion 提供的 jsx 去编译 JSX!

> Both methods result in the same compiled code. After adding the preset or setting the pragma as a comment, compiled jsx code will use emotion’s jsx function instead of React.createElement.

### @emotion/babel-plugin

配合 New JSX Transform 使用:

[Emotion - Babel Plugin](https://emotion.sh/docs/babel)

> Emotion has an optional Babel plugin that optimizes styles by compressing and hoisting them and creates a better developer experience with source maps and labels.

```json
{
  "presets": [
    [
      "@babel/preset-react",
      { "runtime": "automatic", "importSource": "@emotion/react" }
    ]
  ],
  "plugins": ["@emotion/babel-plugin"]
}
```

### @emotion/babel-preset-css-prop

配合 Classic JSX Transform 使用:

```json
{
  "presets": ["@emotion/babel-preset-css-prop"]
}
```

### Babel Macros

[Emotion - Babel Macros](https://emotion.sh/docs/babel-macros), emotion 模块提供了 `macro`, 自行在工程中引入.

> [Create React App recently added support for Babel Macros](https://reactjs.org/blog/2018/10/01/create-react-app-v2.html) which makes it possible to run Babel transforms without changing a Babel config.

[Zero-config code transformation with babel-plugin-macros · Babel](https://babeljs.io/blog/2017/09/11/zero-config-with-babel-macros)

针对 CRA 这种没有自定义项的方式的

### JSX Pragma

利用 babel 插件的特性: [@babel/preset-react · Babel](https://babeljs.io/docs/en/babel-preset-react#pragma)

这种使用的情况是, 工程不提供配置修改的方式 (perset, plugin, macros), 比如 codesandbox, CRA 老版本 (2.0 之前).
那就得自己多写注释代码了 :)

> Set the jsx pragma at the top of your source file that uses the css prop. This option works best for testing out the css prop feature or in projects where the babel configuration is not configurable (create-react-app, codesandbox, etc.).
> Similar to a comment containing linter configuration, this configures the jsx babel plugin to use the jsx function instead of React.createElement.

```tsx
/** @jsx jsx */
import { jsx } from "@emotion/react";

<div css={{ background: "black" }} />;
```

## 参考资料

- [Let jsxImportSource be configurable through tsconfig.json · Issue #9847 · facebook/create-react-app](https://github.com/facebook/create-react-app/issues/9847)
