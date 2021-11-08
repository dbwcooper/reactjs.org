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
  - auto, 元素将尽可能的去获取父容器的剩余空间。
  - 0, 将被解释为 width:0; 从而将元素折叠到给定内容的最小可能宽度

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
    margin: '4px',
    width: 200,
    border: '1px solid #9CA3AF',
    padding: '6px',
    'justify-content': 'space-between',
  };
  const boxStyle = {flex: 'auto', border: '1px solid #D1D5DB', padding: '10px'};
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
    margin: '4px',
    width: 200,
    border: '1px solid #9CA3AF',
    padding: '6px',
    'justify-content': 'space-between',
  };
  const boxStyle = {flex: 0, border: '1px solid #D1D5DB', padding: '10px'};
  return (
    <div style={style}>
      <div style={boxStyle}>box1</div>
      <div style={boxStyle}>box2</div>
    </div>
  );
}
```

</Sandpack>

## flex 与 margin

flex 子元素可以结合 margin 实现更灵活的布局

- 主轴为横向时，可以配置使用 margin-left, margin-right
- 主轴为纵向时，可以配置使用 margin-top, margin-bottom

<Sandpack>

```js App.js
import Row from './Row.js';
import Column from './Column.js';
export default function App() {
  return (
    <div>
      <Row />
      <Column />
    </div>
  );
}
```

```js Row.js
export default function Row() {
  const style = {
    display: 'flex',
    margin: '4px',
    border: '1px solid #9CA3AF',
    padding: '6px',
    'justify-content': 'space-between',
  };
  const boxStyle = {border: '1px solid #D1D5DB', padding: '10px', flex: 0};
  return (
    <div style={style}>
      <div style={boxStyle}>box1</div>
      <div style={{...boxStyle, 'margin-left': 'auto'}}>box2</div>
      <div style={boxStyle}>box3</div>
    </div>
  );
}
```

```js Column.js
export default function Column() {
  const style = {
    display: 'flex',
    height: 200,
    'flex-direction': 'column',
    margin: '4px',
    border: '1px solid #9CA3AF',
    padding: '6px',
    'justify-content': 'space-between',
  };
  const boxStyle = {border: '1px solid #D1D5DB', padding: '10px', flex: 0};
  return (
    <div style={style}>
      <div style={boxStyle}>box1</div>
      <div style={{...boxStyle, 'margin-top': 'auto'}}>box2</div>
      <div style={boxStyle}>box3</div>
    </div>
  );
}
```

</Sandpack>

## flex 的计算规则

flex 有一些隐形的计算规则， flex-grow, flex-shrink 有不同的计算规则.

> 因为子元素未撑满容器，使用 flex-grow 规则
> 权重计算公式： (200 - (子元素所有的 flex-basis 和)) / (子元素 flex-grow 和)
> 权重 = 100/3 （auto === 0）
> box1 = 1 \* 100/3 = 33.4
> box2 = 2 \* 100/3 = 33.6
> box3 = 3 \* 100/3 = 100

<Sandpack>

```js App.js active
export default function App() {
  const style = {
    display: 'flex',
    width: 200,
    margin: '4px',
    border: '1px solid #9CA3AF',
  };

  const itemStyle = {
    flex: 1,
    'border-right': '1px solid #D1D5DB',
    width: 40,
  };
  return (
    <div style={style}>
      <div style={{...itemStyle}}>40</div>
      <div style={{...itemStyle, width: 40, flex: 2}}>40</div>
      <div style={{...itemStyle, width: 40, flex: 3}}>40</div>
    </div>
  );
}
```

</Sandpack>

> 因为子元素已经溢出了容器实际大小，使用 flex-shrink 规则
> 预期宽度: 100 \* 1 + 100 \* 1 + 100 \* 2 = 400;
> 溢出宽度: 100 + 100 + 100 - 200 = 100;
> box1: 100 - (100 \* 1)/400 \* 100 = 75;
> box2: 100 - (100 \* 2)/400 \* 100 = 50;
> box3: 100 - (100 \* 1)/400 \* 100 = 75;

<Sandpack>

```js App.js active
import FlexBasisAuto from './flex-basis-auto.js';
import FlexBasis0 from './flex-basis-0.js';
export default function App() {
  return (
    <div>
      <FlexBasisAuto />
      <FlexBasis0 />
    </div>
  );
}
```

```js flex-basis-auto.js
export default function App() {
  const style = {
    display: 'flex',
    width: 200,
    margin: '4px',
    border: '1px solid #9CA3AF',
  };

  const itemStyle = {
    flex: 1,
    borderRight: '1px solid #D1D5DB',
    width: 100,
  };
  return (
    <div style={style}>
      <div style={{...itemStyle, flex: '1 1 auto'}}>100</div>
      <div style={{...itemStyle, flex: '1 2 auto'}}>100</div>
      <div style={{...itemStyle, flex: '2 1 auto', borderRight: 'none'}}>
        100
      </div>
    </div>
  );
}
```

```js flex-basis-0.js
export default function App() {
  const style = {
    display: 'flex',
    width: 200,
    margin: '4px',
    border: '1px solid #9CA3AF',
  };

  const itemStyle = {
    flex: 1,
    borderRight: '1px solid #D1D5DB',
    width: 100,
  };
  return (
    <div style={style}>
      <div style={{...itemStyle, flex: '1 1 0'}}>100</div>
      <div style={{...itemStyle, flex: '1 2 0'}}>100</div>
      <div style={{...itemStyle, flex: '2 1 0', borderRight: 'none'}}>
        100
      </div>
    </div>
  );
}
```

</Sandpack>
