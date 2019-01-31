# css.draft.v1
## CSS 方法论
CSS 理论和框架有不少。但 CSS 框架的用法较为繁琐，而且必须成套使用，而 CSS 理论更多的是阐释 HTML 和 CSS 之间的关系，而不是预编译的代码库，因此使用起来更为灵活。

没有哪个方法论是完美的，一个项目可能与某一个方法契合得最好，但是另一个项目却更适合用另一个方法。所以，完全可以创造自己的方法论，或者将一个现有的理论根据自己的需求进行改造。因此，如果不知道如何选择方法论，最好是看一些比较杰出的方法论，根据你手头的项目来分析其中哪些可用，哪些不可用。下面来看下先比较出名的方法论。

### OOCSS 方法
<details>
<summary></summary>

全称是 Object-Oriented CSS，面向对象的 CSS。看下面示例：
```html
<div class="toggle simple">
  <div class="toggle-control open">
    <h1 class="toggle-title">Title 1</h1>
  </div>
  <div class="toggle-details open"></div>
</div>
```
OOCSS 有两个主要的原则：
- 分离结构和外观：将视觉特性定义为可复用的单元。上面的例子就是一个片段，例如，当前的“simple”皮肤使用直角，而“complex”皮肤可能使用圆角，还加了阴影。
- 分离容器和内容：指的是不再将元素位置作为样式的限定词。和在容器内标记的CSS 类名不同，我们现在使用的是可复用的CSS 类名，如toggle-title， 它应用于相应的文本处理上，而不管这个文本的元素是什么。

当想提供一套组件让开发人员组合起来创建用户界面时，这种方法非常有用。Bootstrap 就是一个特别好的例子，它是一个自带各种皮肤的小组件系统。
</details>


### MACSS 方法
<details>
<summary></summary>

全称是 Scalable and Modular Architecture for CSS，模块化架构的可扩展 CSS。看下面示例：
```html
<div class="toggle toggle-simple">
  <div class="toggle-control is-active">
    <h1 class="toggle-title">Title 1</h1>
  </div>
  <div class="toggle-details is-active"></div>
</div>
```
SMACSS（https://smacss.com/）和 OOCSS 有许多相似之处，但 SMACSS 的不同点是把样式系统划分为五个具体类别。
- 基础：如果不添加CSS 类名，标记会以什么外观呈现。
- 布局：把页面分成一些区域。
- 模块：设计中的模块化、可复用的单元。
- 状态：描述在特定的状态或情况下，模块或布局的显示方式。
- 主题：一个可选的视觉外观层，可以让你更换不同主题。

在例子中，可以看到模块样式（toggle、toggle-title、toggle-details）、子模块（toggle-simple）和状态（is-active）的组合。对于如何创建功能的小模块，OOCSS 和 SMACSS 有许多相似之处。它们都把样式作用域限定到根节点的CSS 类名上，然后通过皮肤（OOCSS）或者子模块（SMACSS）进行修改。除了SMACSS 把代码划分类别之外，两者之间最显著的差异是使用皮肤而不是子模块，以及带 is 前缀的状态类名。
</details>


### BEM 方法
<details>
<summary></summary>

全称是Block Element Modifier，块元素修饰符。看下面示例：
```html
<div class="toggle toggle--simple">
  <div class="toggle__control toggle__control--active">
    <h1 class="toggle__title">Title 1</h1>
  </div>
  <div class="toggle__details toggle__details--active"></div>
</div>
```
BEM 只是一个 CSS 类名命名规则。它不涉及如何书写 CSS 的结构，而只是建议每个元素都添加带有如下内容的 CSS 类名：
- 块名：所属组件的名称。
- 元素：元素在块里面的名称。
- 修饰符：任何与块或元素相关联的修饰符。

BEM 使用非常简洁的约定来创建CSS 类名，而这些字符串可能会相当长。

这种方法在OOCSS 或SMACSS 里使用的好处是，每一个CSS 类名都详细地描述了它实现了什么。代码中没有open 或者is-active 这样只在特定背景下才能理解的CSS 类名。
如果单独看open 和is-active 这两个名字，我们并不知道它们的含义是什么。

虽然 BEM 法看起来很累赘、很冗余 ，但是当看到一个toggle__details--active 的 CSS 类名，就知道它是表示：这个元素的名称是details，位置在toggle 组件里，状态为激活。
</details>


## thy 提议
<details>
<summary></summary>

### 1 首先要文档化，把公用的样式进行说明，例如文件路径。方便寻找。写样式时，优先考虑使用公用样式。

### 2 一个标签上，公用的样式不能超过 3 个，不能达到效果的，再写单独适用的样式名称。

### 3 CSS 选择器的开销从小到大是：
- ID选择符，#exam {}。
- 类选择符, .exam {}。
- 类型选择符，a {}。
- 相邻兄弟选择符，h1 + a {}。
- 子选择符，.exam > a {}。
- 后代选择符，.exam a {}。
- 通配符，* {}。
- 属性选择符，[href="#exam"] {}。
- 伪类和伪元素，a:hover {}
由于CSS 是从右向左进行匹配，所以写 CSS 的建议：
- 避免使用统配规则，也就是不要使用相邻兄弟选择符、子选择符、后代选择符、通配符、属性选择符。
- 避免使用ID选择符。
- 不要使用限定类选择符，例如 li.row 改成 .list-row。
- 越具体越好，不要使用过长后代选择符，例如 .main .content .list .list-row ,直接通过一种规则，指定一个具体的类名，例如 .content-list-row。
- 利用继承。

### 4 后代选择符不能超过 3 层。

### 5 新的公用样式要进行讨论决定。
</details>



