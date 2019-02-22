# cssStandard
## ydh-resource-web 项目维护规范
### <a name="index"></a> 索引
- [1 命名规范](#name)
- [2 原则](#principle)
- [3 代码风格](#style)
- [4 公用样式文件位置](#path)
- [5 评审时关于设计稿需要思考的点](#design)
- [6 写 css 时需要思考的点](#write)
- [7 那些样式该公用](#common)
- [8 成为公用样式条件](#limit)

### <a name="name"></a> 1 命名规范
#### 文件夹名
- 使用小写字母，多个单词用中划线 `-` 连接。

#### 文件名
- css 和 js 文件命名统一使用小写字母，多个单词用驼峰形式，例如 `pageDetail`。

#### class 名
- 以当前 css 文件的名称为第一个单词，该页面下的样式用 BEM 的方式，但只用一个中划线，命名要语义化。
- 小写，多个单词中划线 `-` 连接。
- 如果新写的标签涉及 js 操作需要用到 class，这类 class 需要带 js 前缀，例如 `js-text`。该 class 不能写具体的样式。

#### id 名
- 涉及到大的模块，首字母大写的驼峰形式，命名语义化。
- 不用写 js 前缀。

<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

### <a name="principle"></a> 2 原则
- 不能行内 css。
- css 不能用 id 选择器。
- 样式文件内不能有空的 css 样式。
- 注释的代码要删除。
- 选择器嵌套最多 4 层。
- 添加必要的注释。
- css 书写顺序与结构一致。
- 避免使用标签，例如 `.content ul li`。
- 避免使用通配符。
- 避免使用子选择符，例如 `.content > ul >li`。
- 最后或者第一个不需要的样式效果，可采用伪选择器和 `+` 选择器。
- 没有用 `!important` 的样式，禁止使用 `!important` 覆盖；如果有用，则可以用 `!important` 覆盖。
- 同一类型的公用样式，如果有使用 2 个以上的，则要在其对应自身样式内变成简写，例如标签上有 `mb10 ml10 mr10`，在自身样式内简写为 `margin: 10px 10px 10px 0;`。
- 不能重置公用样式，要么用，要么不使用，自己写一个对应的新样式。

<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

### <a name="style"></a> 3 代码风格
- [缩进](#indent)
- [分号](#semicolon)
- [空格](#space)
- [空行](#empty-row)
- [换行](#line-feed)
- [注释](#note)
- [引号](#quotation)
- [颜色](#color)
- [属性简写](#short)
- [属性声明顺序](#attribute)
- [媒体查询](#media)
- [属性值为 0 的情况](#zero)


#### <a name="indent"></a> 缩进
使用 2 个空格。

#### <a name="semicolon"></a> 分号
每个属性名末尾要加分号。
```css
/* good */
.nav {
  font-size: 18px;
}

/* not good */
.nav {
  font-size: 18px
}
```
<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

#### <a name="space"></a> 空格
需要空格情况：
- `{` 前。
- 冒号 `:` 后面。
- `!important` 前。
- 注释的开始和结尾。

不需要空格情况：
- 冒号 `:` 前面。
- `!important` 中 `!` 前。
- 多个规则的分隔符 `，`前。
- 属性值中 `(` 后和 `)` 前。
```css
/* good */
.nav,
.footer {
  font-size: 18px;
  color: #666 !important;
  background-color: rgba(0,0,0,.5);
}

/*not good*/
.nav ,
.footer{
  font-size :18px;
  color: #666! important;
  background-color: rgba( 0, 0, 0, .5 );
}
```

#### <a name="empty-row"></a> 空行
需要空行的情况：
- 文件最后保留一个空行。
- `}` 后留空行.
```css
/* good */
.nav {
  font-size: 18px;
}

.footer {
  color: #666;
}

/* not good */
.nav {
  font-size: 18px;
}
.footer {
  color: #666;
}
```
<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

#### <a name="line-feed"></a> 换行
需要换行的情况：
- `{` 后和 `}` 前。
- 每个属性及对应值独占一行。
- 多个规则的分隔符 `,` 后。

```css
/* good */
.nav,
.footer {
  font-size: 18px;
  color: #666;
}

.body {
  font-size: 16px;
}

/* not good */
.nav,.footer {
  font-size: 18px;color: #666;
}

.body {font-size: 16px;}
```

#### <a name="note"></a> 注释
- 外层加注释，`{}` 内不能加注释。
```css
/* good */
.footer {
  font-size: 18px;
}

.footer {
  /* not good */
  font-size: 18px;
}

```
<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

#### <a name="quotation"></a> 引号
- 统一使用双引号。
```css
/* good */
.nav:before {
  content: "";
  font-size: 18px;
}

/* not good */
.nav:before {
  content: '';
  font-size: 18px;
}
```

#### <a name="color"></a> 颜色
- 十六进制跟原生交互的时候，用全写，其余情况用简写且小写。
- 避免使用有透明度的颜色值，如果有，使用 `rgba()` 形式，其余情况禁止使用 `rgba()` 形式。
- 颜色值不允许用原生单词值，例如 `red`。

<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

#### <a name="short"></a> 属性简写
- 能简写，尽量简写。如果不熟悉，自己查找资料。

#### <a name="attribute"></a> 属性声明顺序
<details>
<summary>1. 定位布局属性。</summary>

```javascript
[
  "display",
  "visibility",
  "float",
  "clear",
  "overflow",
  "clip",
  "zoom",
  "table-layout",
  "border-spacing",
  "border-collapse",
  "list-style",
  "flex",
  "flex-direction",
  "justify-content",
  "align-items",
  "position",
  "top",
  "right",
  "bottom",
  "right",
  "z-index",
]
```
</details>

<details>
<summary>2. 盒子模型属性。</summary>

```javascript
[
  "margin",
  "box-sizing",
  "border",
  "border-radius",
  "padding",
  "width",
  "min-width",
  "max-widht",
  "height",
  "min-height",
  "max-height",
]
```
</details>

<details>
<summary>3. 文本属性。</summary>

```javascript
[
  "font-size",
  "line-height",
  "text-align",
  "vertical-align",
  "white-space",
  "text-decoration",
  "text-emphasis",
  "text-indent",
  "text-overflow",
  "word-wrap",
  "word-break",
  "color",
  "text-shadow",
]
```
</details>

<details>
<summary>4. 其它属性。</summary>

```javascript
[
  "background",
  "background-color",
  "background-image",
  "background-repeat",
  "background-attachment",
  "background-position",
  "background-clip",
  "background-origin",
  "background-size",
  "outline",
  "opacity",
  "filter",
  "box-shadow",
  "transitio"n
  "transform",
  "animation",
  "cursor",
  "pointer-events",
]
```
</details>

<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

#### <a name="media"></a> 媒体查询
尽量将媒体查询的规则靠近与他们相关的规则，不要将他们一起放到一个独立的样式文件中，或者丢在文档的最底部，这样做只会让大家以后更容易忘记他们。
```css
/* good */
.nav {
  font-size: 14px;
}

@media (min-width: 480px) {
  .nav {
    font-size: 16px;
  }
}
```

#### <a name="zero"></a> 属性值为 0 的情况
- 不要带单位。
- 去除小数点前面的 `0`。

## <a name="path"></a> 4 公用样式文件位置

## <a name="design"></a> 5 评审时关于设计稿需要思考的点
- 评审或直接跟设计交流的时候，发现问题要及时提出。对于容易反复出现的问题点，每次坚持提示 1 到 2 个改进点，例如提醒设计是不是用已有的颜色值，不要用有透明度的颜色值等等。

## <a name="write"></a> 6 写 css 时需要思考的点
- 如果要写新样式，发现在已有公用样式中没有，则要首先跟设计交流，确认是否新增样式。最好使用已有颜色相近的颜色值。
- 已有公用样式，先不动，能用则用。

<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

## <a name="common"></a> 7 那些样式该公用
- 色值
- 字体大小
- 布局
- 边距
- 状态（例如文字水平居中）

## <a name="limit"></a> 8 成为公用样式条件
- 新增的样式使用地方达到 `5` 处，则抽出公用样式，此后碰到这种样式就用该公用样式。

注意在这里的样式是要经过第 5、6、7点思考后的样式。