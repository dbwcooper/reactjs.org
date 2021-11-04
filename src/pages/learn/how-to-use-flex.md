---
title: How to use flex
---

## 基础信息

flex 是一个二维布局系统，有横向和纵向两个布局方向。
参考 [flex 布局系统](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout)

<Sandpack>

```js App.js active
import FlexRow from './FlexRow.js';
import FlexColumn from './FlexColumn.js';
export default function App() {
  return (
    <>
      <FlexRow />
      <FlexColumn />
    </>
  );
}
```

```js FlexRow.js active
const style = {
  display: 'flex',
  width: 200,
  border: '1px solid #9CA3AF',
  margin: '4px',
  padding: '6px',
  'justify-content': 'space-between',
};
export default function FlexRow() {
  return (
    <div style={style}>
      <div style={{border: '1px solid #D1D5DB', padding: '10px'}}>box1</div>
      <div style={{border: '1px solid #D1D5DB', padding: '10px'}}>box2</div>
    </div>
  );
}
```

```js FlexColumn.js
const style = {
  display: 'flex',
  'flex-direction': 'column',
  margin: '4px',
  width: 200,
  border: '1px solid #9CA3AF',
  padding: '6px',
  'justify-content': 'space-between',
};
export default function FlexColumn() {
  return (
    <div style={style}>
      <div style={{border: '1px solid #D1D5DB', padding: '10px'}}>box1</div>
      <div style={{border: '1px solid #D1D5DB', padding: '10px'}}>box2</div>
    </div>
  );
}
```

</Sandpack>

## flex 属性特点

- flex 属性 **一位** 表示法

  - 值为数字时，表示 flex-grow,
  - 值为非数字时，表示 flex-basic

  ```css
  flex: 0; // flex: 0 1 0%;
  flex: 1; // flex: 1 1 0%;
  flex: auto; // flex: 1 1 auto;
  flex: 10px; // flex: 0 1 10px;
  ```

- flex 属性 **二位** 表示法

  - 第一个值为 flex-grow
  - 第二个值为数字时表示 flex-shrink, 非数字时表示 flex-basic

  ```css
  flex: 0 auto; === flex 0 1 auto;
  flex: 1 2; === flex 1 2 0%;
  flex: 2 10px; === flex 2 1 10px;
  ```

- flex-basic 不同值的区别
  - auto
  - 0%

<Sandpack>

```js App.js active
import FlexBasicAuto from './FlexBasic-auto.js';
import FlexBasic0 from './FlexBasic-0.js';
export default function App() {
  return (
    <>
      <FlexBasicAuto />
      <FlexBasic0 />
    </>
  );
}
```

```js FlexBasic-auto.js
export default function FlexBasicAuto() {
  const style = {
    display: 'flex',
    'flex-direction': 'column',
    margin: '4px',
    width: 200,
    border: '1px solid #9CA3AF',
    padding: '6px',
    'justify-content': 'space-between',
  };
  const boxStyle = {border: '1px solid #D1D5DB', padding: '10px', flex: 'auto'};
  return (
    <div style={style}>
      <div style={boxStyle}>box1</div>
      <div style={boxStyle}>box2</div>
    </div>
  );
}
```

```js FlexBasic-0.js
export default function FlexBasic0() {
  const style = {
    display: 'flex',
    'flex-direction': 'column',
    margin: '4px',
    width: 200,
    border: '1px solid #9CA3AF',
    padding: '6px',
    'justify-content': 'space-between',
  };
  const boxStyle = {border: '1px solid #D1D5DB', padding: '10px', flex: 0};
  return (
    <div style={style}>
      <div style={boxStyle}>box1</div>
      <div style={boxStyle}>box2</div>
    </div>
  );
}
```

</Sandpack>

## flex 的计算规则

flex 有一些隐形的计算规则， 在对 flex 子项进行 flex-grow, flex-shrink 分派时，有不同的计算规则

- Start with a **minimal set up with just a toolchain,** adding features to your project as necessary.
- Start with an **opinionated framework** with common functionality already built in.

Whether you're just getting started, looking to build something big, or want to set up your own toolchain, this guide will set you on the right path.

## Getting started with a React toolchain

If you're just getting started with React, we recommend [Create React App](https://create-react-app.dev/), the most popular way to try out React's features and a great way to build a new single-page, client-side application. Create React App is an unopinionated toolchain configured just for React. Toolchains help with things like:

- Scaling to many files and components
- Using third-party libraries from npm
- Detecting common mistakes early
- Live-editing CSS and JS in development
- Optimizing the output for production

You can get started building with Create React App with one line of code in your terminal! (**Be sure you have [Node.js](https://nodejs.org/) installed!**)

<TerminalBlock>

npx create-react-app my-app

</TerminalBlock>

Now you can run your app with:

<TerminalBlock>

cd my-app
npm start

</TerminalBlock>

For more information, [check out the the official guide](https://create-react-app.dev/docs/getting-started).

> Create React App doesn't handle backend logic or databases; it just creates a frontend build pipeline. This means you can use it with any backend you want. But if you're looking for more features like routing and server-side logic, read on!

### Other options

Create React App is great to get started working with React, but if you'd like an even lighter toolchain, you might try one of these other popular toolchains:

- [Vite](https://vitejs.dev/guide/)
- [Parcel](https://parceljs.org/)
- [Snowpack](https://www.snowpack.dev/tutorials/react)

## Building with React and a framework

If you're looking to start a bigger, production-ready project, [Next.js](https://nextjs.org/) is a great place to start. Next.js is a popular, lightweight framework for static and server‑rendered applications built with React. It comes pre-packaged with features like routing, styling, and server-side rendering, getting your project up and running quickly.

[Get started building with Next.js](https://nextjs.org/docs/getting-started) with the official guide.

### Other options

- [Gatsby](https://www.gatsbyjs.org/) lets you generate static websites with React with GraphQL.
- [Razzle](https://razzlejs.org/) is a server-rendering framework that doesn't require any configuration, but offers more flexibility than Next.js.

## Custom toolchains

You may prefer to create and configure your own toolchain. A JavaScript build toolchain typically consists of:

- A **package manager**—lets you install, updated and manage third-party packages. [Yarn](https://yarnpkg.com/) and [npm](https://www.npmjs.com/) are two popular package managers.
- A **bundler**—lets you write modular code and bundle it together into small packages to optimize load time. [Webpack](https://webpack.js.org/), [Snowpack](https://www.snowpack.dev/), [Parcel](https://parceljs.org/) are several popular bundlers.
- A **compiler**—lets you write modern JavaScript code that still works in older browsers. [Babel](https://babeljs.io/) is one such example.

In a larger project, you might also want to have a tool to manage multiple packages in a single repository. [Nx](https://nx.dev/react) is an example of such a tool.

If you prefer to set up your own JavaScript toolchain from scratch, [check out this guide](https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658) that re-creates some of the Create React App functionality.
