## 命名

### 1. 声明式命名

```css
/* 不推荐 */
.box1 {
  color: red;
}
.box2 {
  color: blue;
}
.red {
}

/* 推荐 */
.box {
}

.block {
}

.panel {
}
```

### 2. 建议用 `-` 连接，名字不要过长超过 4 个连接符

这个不同的规范有不同的写法，没有优劣之分，可以统一用一种书写方式。

### 3. 如果是从属关系，在类名上体现出来

```less
// 不推荐
.list {
  width: 100px;
  .item {
    margin-bottom: 10px;
  }
}
// 推荐
.list {
  width: 100px;
  .list-item {
    margin-bottom: 10px;
  }
}
```

### 4. 尽量少用id

一般情况下ID不应该被用于样式，并且ID的权重很高，所以不使用ID解决样式的问题，而是使用class。且id命名会注册为全局变量，造成命名污染。

### 5. css选择器中避免使用标签名
例子：
```html
<div>
  <p>
    hello, <span> world</span> !
  </p>
</div>
```
```css
div p span {
  color:red;
}
```
当使用标签名来写样式时,当p标签里多加入一些span标签，或者子孙标签中还有span标签，都会被作用到。这可能并不是我们想要的效果。建议用类名控制，辅以子选择器 `>`。



## React样式规范
### 1. 组件样式文件名与组件名一致
```
|- Tree.js
|- Tress.less
```

### 2. 单页面不要给组件加一级类

当我们给一些组件加一级样式时，这个组件可能被公用，单页在加载时，A 页面卸载后其样式可能还继续生效，会导致 B 页的组件样式被污染。最好用一个大类包裹起来，防止污染。

例如：

Detail.jsx

```jsx
import './style.less';

<Page class='detail-page'>
  <Header />
  <Body class='container'>
    <Modal class='tips' />
  </Body>
  <Footer />
</Page>;
```

style.less

```less
// 不推荐
.tips {
  backgroud-color: red;
}

// 建议
.detail-page {
  .container {
    .tips {
      backgroud-color: red;
    }
  }
}
```


## 动态样式特性

### 1. 继承语法

> 使用 less、scss 的预编译语法，可以使用继承语法，一次定义多次使用

less 语法

```less
// 编译前
.border {
  border: 1px solid #ccc;
  border-radius: 3px;
  box-shadow: 3px 3px 2px rgba(0, 0, 0, 0.2);
}

.box {
  &:extend(.border);
  background: blue;
}

// 编译后为
.border {
  border: 1px solid #ccc;
  border-radius: 3px;
  box-shadow: 3px 3px 2px rgba(0, 0, 0, 0.2);
}
.box {
  border: 1px solid #ccc;
  border-radius: 3px;
  box-shadow: 3px 3px 2px rgba(0, 0, 0, 0.2);
  background: blue;
}
```

## 其他
### 1. 其他的书写规范，可以统一一份stylelint。
```
1. 0后面不带单位
2. 2个空格缩进
3. 类名后空格
4. 多个并行的类要换行
5. 属性要换行
```
### 2. css 嵌套不要超过 4 层，最好控制在 3 层以内。嵌套过多会影响渲染性能


## 疑问
### 1. 设计稿色值与已定义变量名不一致，如何取色？


## 参考链接

1. [SMACSS](https://segmentfault.com/a/1190000005123938#articleHeader5)

2. [css编码规范](https://github.com/fex-team/styleguide/blob/master/css.md)
