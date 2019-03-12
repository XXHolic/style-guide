# jsStandard

## h5 管理端维护规范

### 命名规范

#### 目录命名

**非 react 项目**

- 全部采用小写方式，多个单词，用驼峰式，例如 `pageDetail`。

**react 项目**

- 建议小写方式，以中划线分割`-`。 例如：`material-ui-design`

#### 文件命名

- 组件采用大写驼峰命名, 即帕斯卡命名。 例如： `PageHeader`。
  组件名与文件名一致。

```js
// bad
import Footer from './Footer/Footer';

// bad
import Footer from './Footer/index';

// good
import Footer from './Footer';
```

### 目录规范

- page 下目录不能超过 3 层，包含 page 目录。

### 原则

- 每个文件只包含一个 React 组件
- 使用 JSX 语法
- 不要使用 React.createElement，以`Pure Function`、`class`组件代替
- 高阶组件
  但构建一个高阶组件时，我们给返回的组件生成一个`dispalyName`。例如：高阶组件为`withFoo`,需要被高阶组件包裹的组件为`Bar`，需要返回一个`displayName`为 `withFoo(Bar)`.
  > Why? A component’s displayName may be used by developer tools or in error messages, and having a value that clearly expresses this relationship helps people understand what is happening.

```jsx
// bad
export default function withFoo(WrappedComponent) {
  return function WithFoo(props) {
    return <WrappedComponent {...props} foo />;
  }
}

// good
export default function withFoo(WrappedComponent) {
  function WithFoo(props) {
    return <WrappedComponent {...props} foo />;
  }

  const wrappedComponentName = WrappedComponent.displayName
    || WrappedComponent.name
    || 'Component';

  WithFoo.displayName = `withFoo(${wrappedComponentName})`;
  return WithFoo;
}
```

- props 命名：避免个 html 属性名一致

```jsx
// bad
<MyComponent style="fancy" />

// bad
<MyComponent className="fancy" />

// good
<MyComponent variant="fancy" />
```

### 代码风格

#### 缩进

- 使用 2 个空格。

#### 分号

以下几种情况后需加分号：

- 变量声明
- 表达式
- return
- throw
- break
- continue
- do-while

#### 空格

参照 `thy`

#### 空行

参照 `thy`

#### 换行

参照 `thy`

#### 注释

#### 引号

#### 括号

#### 变量

#### 对象

#### 数组

#### 函数

#### 字符串

#### 原型

#### 模块

#### 迭代器

### 参照

1. [Airbnb React 规范](https://github.com/airbnb/javascript/tree/master/react#basic-rules)
