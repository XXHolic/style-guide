# css.draft.thy
## 思路
<details>
<summary>一个新项目，该如何组织？</summary>

- 基于个人经验，总结出的自己的一套。
- 直接拿网上已有的规范。
- 基于个人、网上和实际情况弄一套。

</details>

<details>
<summary>基于已有项目重构，该如何组织？</summary>

- 先了解现有项目大体情况，根据情况选择重构方案。

</details>

<details>
<summary>基于已有项目维护，该如何组织？</summary>

- 划定明确的新旧界线，基于已有的样式制定明确的规范。

</details>

## CSS 方法论
谈到 CSS 就离不开 HTML，现在有不少关于阐释 HTML 和 CSS 之间关系的理论。了解这些，对于组织 CSS 很有帮助。下面通过网上的一些资料，介绍一些认可度比较高的方法论。

### OOCSS 方法
<details>
<summary></summary>

全称是 Object-Oriented CSS，面向对象的 CSS。在 2008 年的时候就被提出，在 [OOCSS wiki][url-oocss-wiki] 介绍中，该方法有两个主要的原则：
#### 1 分离结构和外观
意思是将视觉特性定义为可复用的单元（例如背景和边框样式）。换句话说，就是把布局和设计的样式都独立出来。见下面的例子：
```html
<div class="warn">warning</div>
```
样式 warn 表示警告的字体颜色，在系统内，只要是用到警告的地方，统一使用这个样式。

#### 2 分离容器和内容
意思是指把内容从容器中分离开来。不论你放到那里，看起来都是一样的。见下面的例子：
```html
<div class="dialog blue">
  <div class=“dialog-title”><span class="text">Title</span></div>
  <div class="dialog-content show"><span class="text">content</span></div>
</div>
```
这个例子中不同的模块显示不同的字体和颜色，一般可能会这样做：
```css
.dialog-title .text {color: #333;}
.dialog-content .text {color: #666;}
```
但在 OOCSS 方法中是通过添加一个新的 `class` 来描述需要的样式：
```html
<div class="dialog blue">
  <div class=“dialog-title”><span class="text title-text">Title</span></div>
  <div class="dialog-content show"><span class="text content-text">content</span></div>
</div>
```
```css
.title-text {color: #333;}
.content-text {color: #666;}
```
当想提供一套组件让开发人员组合起来创建用户界面时，这种方法非常有用。Bootstrap 就是一个例子，它是一个自带各种皮肤的小组件系统。
</details>


### SMACSS 方法
<details>
<summary></summary>

全称是 Scalable and Modular Architecture for CSS，模块化架构的可扩展 CSS。[SMACSS][url-smacss] 把样式系统划分为五个具体类别：
- 基础：标签默认展示的样式，基本上就是元素选择器，可以包含属性选择器、伪类选择器、子选择器或兄弟选择器。在这个里面不应该出现 `!important` 。
- 布局：把页面分成一些区域。
- 模块：设计中的模块化、可复用的单元，例如列表、弹窗等。
- 状态：描述在特定的状态或情况下，模块或布局的显示方式。
- 主题：一个可选的视觉外观层，可以让你更换不同主题。

见下面的例子：
```html
<div class="dialog dialog-blue">
  <div class=“dialog-title”><span class="text">Title</span></div>
  <div class="dialog-content is-show"><span class="text">content</span></div>
</div>
```
观察模块样式 `dialog`、`dialog-blue`、`dialog-title`、`dialog-content` 和状态 `is-show` ，可以发现对于如何创建功能的小模块，OOCSS 和 SMACSS 有许多相似之处。它们都把样式作用域限定到根节点的 CSS 类名上，然后通过皮肤（OOCSS）或者子模块（SMACSS）进行修改。除了SMACSS 把代码划分类别之外，两者之间最显著的差异是使用皮肤而不是子模块，以及带 `is` 前缀的状态类名。
</details>


### BEM 方法
<details>
<summary></summary>

全称是 Block Element Modifier，块元素修饰符。[BEM][url-BEM] 是一种基于组件的 Web 开发方法。其背后的想法是将用户界面划分为独立的块。其中包含的内容不仅仅是 CSS，其中 CSS 的内容结合[About HTML semantics and front-end architecture][url-blog1]，得出一套命名规则：
- 块名：所属组件的名称，一般形式为 `.block`。
- 元素：元素在块里面的名称，一般形式为 `.block__element`。
- 修饰符：任何与块或元素相关联的修饰符,一般形式为 `.block--modifier`。

看下面例子：
```html
<div class="dialog dialog-skin-blue">
  <div class=“dialog__title”><span class="dialog__text">Title</span></div>
  <div class="dialog__content dialog__content--show"><span class="dialog__text">content</span></div>
</div>
```
这种方式不建议使用元素选择器，名称过长时，用 - 连接。BEM 使用非常简洁的约定来创建 CSS 类名，而这些字符串可能会相当长。

这种方法在 OOCSS 或 SMACSS 里使用的好处是，每一个 CSS 类名都详细地描述了它实现了什么。代码中没有 `show` 或者 `is-show` 这样只在特定背景下才能理解的 CSS 类名。如果单独看 `show` 或者 `is-show` 这两个名字，我们并不知道它们的含义是什么。

虽然 BEM 法看起来很累赘、很冗余 ，但是当看到一个 `dialog__content--show` 的 CSS 类名，就知道它是表示：这个元素的名称是 content，位置在 dialog 组件里，状态为显示。
</details>

## <a name="choose"></a> 关于方法的选择
<details>
<summary></summary>

没有什么方法论是完美的，你可能会发现，不同的项目适合不同的方法。也许还有新的方法，我们还没有发现，不要因为一套规范很流行或者别的团队在使用就选择它。理解方案背后这么做的原因，不要害怕尝试，混合已有的方案，甚至自己或者团队一起创造出独一无二的方案。
</details>


## 规范提议
<details>
<summary>原则</summary>

- 删除僵尸代码。
- 不能有行内 CSS。
- 不能有空的规则。
- 选择器不能超过 3 层。
- 一个标签上的类名不能超过 4 个，给 js 使用的类名不算。
- 若无 ID 可用，给 js 使用的类样式必须带 js 前缀，且不能有具体样式。
- 删除冗余 ID，避免使用 ID 选择器，如果有，转换为类选择器。
- 给出必要的注释说明。
- 抽离基础样式和功能性样式，并提供注释说明，对团队告知对应位置。
- 避免使用 `!important`。

由于 CSS 选择符是从右到左进行匹配，所以：
- 避免使用标签，例如 `.content ul li`。
- 避免使用通配符。
- 避免使用子选择符，例如 `.content > ul >li`。
</details>

<details>
<summary>命名</summary>

- 根据选择的方法，与团队成员共同商定命名形式，可能涉及到公用样式、组件样式、对应页面样式。
- 类名使用小写。
- ID 采用驼峰式命名。

需要更具不同的项目，具体的指定。
- ydh-resource-web
- ircloud-ydh-web
- ircloud-ydh-h5
</details>

<details>
<summary><a name="index"></a> 代码风格</summary>

- [缩进](#indent)
- [空格](#space)
- [空行](#empty-row)
- [换行](#line-feed)
- [分号](#semicolon)
- [引号](#quotation)
- [颜色](#color)
- [属性值为 0 的情况](#zero)
- [属性简写](#short)
- [属性声明顺序](#attribute)
- [媒体查询](#media)

<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

#### <a name="indent"></a> 缩进
使用 2 个空格。

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
<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

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
<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

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

#### <a name="quotation"></a> 引号
统一使用双引号。
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
<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

#### <a name="color"></a> 颜色
使用小写字幕，能简写就使用简写。
```css
/* good */
.nav {
  color: #ab1243;
  background-color: #236;
}

/* not good */
.nav {
  color: #AB1243;
  background-color: #223366;
}
```

#### <a name="zero"></a> 属性值为 0 的情况
- 不要带单位。
- 在定义无边框样式时，使用 0 代替 none。
- 去除小数点前面的 0。

<div align="right"><a href="#index">Back to top :arrow_up:</a></div>

#### <a name="short"></a> 属性简写
需要简写的属性有：
- margin。
- padding。
```css
/* good */
.nav {
  margin: 10px 0 0 6px;
  padding: 2px 0 0 3px;
}

/* not good */
.nav {
  margin-top: 10px;
  margin-left: 6px;
  padding-top: 2px;
  padding-left: 3px;
}
```
#### <a name="attribute"></a> 属性声明顺序
建议顺序为：
<details>
<summary>1. 布局定位属性。</summary>

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
<summary>2. 自身属性。</summary>

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
</details>


## ydh-resource-web
### 现有公用样式


### 命名规范
<details>
<summary></summary>

#### 文件名
- 使用小写字母。

#### class 名
- 类名使用小写，用中划线连接。

#### id 名
- 采用驼峰式命名。
</details>

### 原则
<details>
<summary></summary>

- 删除僵尸代码。
- 不能有行内 CSS。
- 不能有空的规则。
- 选择器不能超过 3 层。
- 一个标签上的类名不能超过 4 个，给 js 使用的类名不算。
- 若无 ID 可用，给 js 使用的类样式必须带 js 前缀，且不能有具体样式。
- 删除冗余 ID，避免使用 ID 选择器，如果有，转换为类选择器。
- 给出必要的注释说明。
- 抽离基础样式和功能性样式，并提供注释说明，对团队告知对应位置。
- 避免使用 `!important`。

由于 CSS 选择符是从右到左进行匹配，所以：
- 避免使用标签，例如 `.content ul li`。
- 避免使用通配符。
- 避免使用子选择符，例如 `.content > ul >li`。
</details>


### 代码风格
为了方便对比统计，有以下基本分类，如果觉得有其它必要的，可以另行在自己文档中补充新的分类。觉得没有必要的部分，跳过即可。

#### 缩进
使用 2 个空格。

#### 分号
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

#### 空格
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

#### 空行
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

#### 换行
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

#### 注释

#### 引号
统一使用双引号。
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

#### 颜色
使用小写字幕，能简写就使用简写。
```css
/* good */
.nav {
  color: #ab1243;
  background-color: #236;
}

/* not good */
.nav {
  color: #AB1243;
  background-color: #223366;
}
```

#### 属性简写

#### 属性声明顺序

#### 媒体查询

#### 属性值为 0 的情况
- 不要带单位。
- 在定义无边框样式时，使用 0 代替 none。
- 去除小数点前面的 0。


## <a name="reference"></a> 参考资料
<details>
<summary></summary>

- [Airbnb CSS / Sass Styleguide](https://github.com/airbnb/css)
- [Code Guide by @AlloyTeam](http://alloyteam.github.io/CodeGuide/)
- [前端代码规范](https://guide.aotu.io/docs/)
- [Google HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html)
- [高性能网站建设进阶指南](https://book.douban.com/subject/4719162/)
- [前端架构设计](http://www.ituring.com.cn/book/1946)
- [CSS 重构](http://www.ituring.com.cn/book/1943)
- [60 CSS Coding Style Guide Examples For Developers](https://techfragments.com/css-style-guide-examples/)
- [CSS 设计指南](http://www.ituring.com.cn/book/1111)
</details>



[url-oocss]:http://oocss.org/
[url-oocss-wiki]:https://github.com/stubbornella/oocss/wiki
[url-smacss]:https://smacss.com/
[url-bem]:https://en.bem.info/
[url-blog1]:http://nicolasgallagher.com/about-html-semantics-front-end-architecture/



