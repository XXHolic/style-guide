# css规范
## 命名规范
### 类名，id名为小写字母组成，用-链接
### 页面采用BEM命名法：block-element-modifier
``` 
.user-home-nav  
　.user-home-nav-item  
　　.user-home-nav-item-icon  
　　.user-home-nav-item-text
```

在scss中
``` scss
.user-home-nav {
    &-item {
        &-icon {}
        &-text {}
    }
}
```
BEM使代码重用性降低，但利于维护，便于维护带来的好处绝对超过代码冗余带来的坏处。页面样式重用性较低，比较适合用BEM规范
BEM命名范例：
``` html
<div class="product-detail">
    <div class="product-detail-header">
        //此TXT和body中txt可共用
        <div class="product-detail-txt"></div>
        //当名称过长的时候也最好确保是BEM格式
        <div class="detail-header-status"></div>
        //带有modifier的class
        <div class="detail-header-status--success"></div>
        <div class="detail-header-status--error"></div>
    </div>
    <div class="product-detail-body">
         //此TXT和body中txt可共用
         <div class="product-detail-txt"></div>
         //当名称过长的时候也最好确保是BEM格式 (BEM格式并不是要求只有一个B一个E)
         <div class="detail-body-status"></div>
         //带有modifier的class
         <div class="detail-body-status--active"></div>
         <div class="detail-body-status--noactive"></div>
   </div>
</div>
```
### 组件样式，公共样式，模块内公共样式推荐加入命名空间，避免冲突。

1、公共样式以 g 为命名空间，例如：.g-selected 、.g-clearfix。  
2、组件样式以（ui,或者其他）为命名空间，例如：.ui-select,.ui-button。  
3、模块公共组件以m为命名空间，例如：.m-product-card,.m-product-picture。

### 数值与单位
1、当单位为0时省略单位。  
2、当单位为(1,0)范围小数时，省略小数前0，如.2,.34。  
3、当十六进制的颜色属性值使用小写和尽量简写：~~#ffffff~~(#fff) ,~~#ffdd22~~ (#fd2)。
### 使用 scss 、 less 等预处理器时，建议嵌套层级不超过 3 层。

### 尽量避免使用标签名
使用 scss ，或者说在 css 里也有这种困惑。假设我们有如下 html 结构：
``` html
<div class="g-content">
    <ul class="g-content-list">
        <li class="item"></li>
        <li class="item"></li>
        <li class="item"></li>
        <li class="item"></li>
    </ul>
</div>
```
在给最里层的标签命名书写样式的时候，我们有两种选择：
``` scss
.g-content {
  .g-content-list {
    li {
    
    }
  }
}
```
或者是
``` scss
.g-content {
  .g-content-list {
    .item {
      
    }
  }
}
```
也就是，编译之后生成了下面这两个，到底使用哪个好呢？

.g-content .g-content-list li { }  
.g-content .g-content-list .item { }  
>基于 CSS 选择器的解析规则（从右向左），建议使用上述第二种 .g-content .g-content-list .item { } ，避免使用通用标签名作为选择器的一环可以提高 CSS 匹配性能。

>浏览器的排版引擎解析 CSS 是基于从右向左（right-to-left）的规则，这么做是为了使样式规则能够更快地与渲染树上的节点匹配。







