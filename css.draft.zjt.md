## css 规范
* **可能产生疑问的**
  * **业务过程中产生的css**
    1. 在新的业务过程中会产生新的css,有可能会使公用的样式丢失，可能ui也不记得这块是按照组件规范的，现在迭代过程中样式可能不通用了，这种情况下应该重写还是在通过在该功能模块在外层父级加class直接覆盖掉原有样式，该功模块又有多个css文件修饰，新的样式又该写在哪个文件夹下
    2. 之前的页面css命名不规范，新创建的页面又该以何种规范命名，比如功能模块，驼峰还是下划线，嵌套过深的话，链式写法最多几层。
  * **h5订货端的css**
    1. 这个项目目前并没有用css预编译语言，是否需要和其它的react项目某些保持一致，比如每书写一个元素的需要空一行({}后面)
    ```javascript {.line-numbers}
          .class-a{
            color: #ffffff;
          }

          .class-b {
            position: sticky;
          }
    ```
    2. 每个页面是否需要新的css文件
* **书写规范**
  1. 命名尽量语义化，像html一样
  2. 如果采用a-b的写法，建议最多4层
  3. class单词过长采用简写
  4. 16进制的颜色不能简写. 比如：#ff6600 => #f60. 如果和原生交互那边貌似识别不了
  5. 不使用id进行css修饰
  6. 分类书写，字体的css写在一起，间距的写在一起，不能随意拼凑
    ```javascript {.line-numbers}
        {
          font-size: 12px;
          font-family: 'Microsoft';
          padding: 4px;
          margin: 4px;
          width: 100px;
          height: 100px;
        }
    ```
  7. 尽量少使用内联样式，如果需要，在组件外面定义一个style变量，把变量写进去
  8. 对于像列表，右侧需要border,但是最后一个需要的样式，不采用覆盖样式的方法
      <p>例: z | j | t</p>
    ```javascript {.line-numbers}
        // 不推荐                                // 推荐  
        .class-a{                             .class-a + .class-a {
          border-right: 1px solid #ff6600;         border-left: 1px solid #ff6600;
        }                                     }
        .class-a:last-child {
          border: none;
        }
    ```


## ydh-resource-web

### 现有公用样式路径
- agentV2/resource-app/css/init.css // 140-169行框架样式
- agentV2/resource-app/css/lib.css
- agentV2/resource-app/css/app.css // 插件以及全局功能样式，比如弹框、刷新==

<details>
<summary>命名规范</summary>

#### 文件命名
- 使用小写，按功能模块
****
#### class命名
- 采用中划线
- 用于js操作的加上js前缀
- 如果是单纯需要一个盒子用来包裹子元素来使子元素布局的话，需要加后缀wrap
****
#### id命名
- 功能模块
- 驼峰式命名
</details>

<details>
<summary>原则</summary>

- 命名尽量语义化
- `id` 不用于css修饰
- 发现没有用的 `class` 或者 `style` 要及时删掉
- 尽量少用 `!important`
- 选择器超过三层的要适时去掉中间的 `class`
- 对于类似列表，右侧需要border,但是最后一个需要的样式，不采用覆盖样式的方法
      <p>例: z | j | t</p>
    ```css
        // 不推荐                                // 推荐  
        .class-a{                             .class-a + .class-a {
          border-right: 1px solid #ff6600;         border-left: 1px solid #ff6600;
        }                                     }
        .class-a:last-child {
          border: none;
        }
    ```
- 相应功能带上注释
</details>

<details>
<summary>代码风格</summary>

#### 缩进 
- `2` 个字节
****

#### 分号
- 属性结束必须带分号
****

#### 空格
- `:` 后面带空格，前面不需要
- `{` 前面带空格
- 注释前后带空格
****  

#### 空行
- `{` 后面空行
- 模块之间空行
****  

  
#### 换行
- `{` 和 `}` 后需要换行
- `,` 后面换行
- 属性之间后面即`;`后换行
****  

####　注释
- 额外或特殊单独的css注释
****  

#### 引号
****  

#### 颜色
- 采用16进制，小写字母，可简写的简写
- 如果与原生交互不能简写，要写注释
****  

#### 属性简写
- `margin`、`padding`超过2个采用复合写法，反之简写
- `transition`、`animation`等css3属性采用简写
****  

#### 属性顺序说明
****  

#### 媒体查询
跟随用到的功能模块方便查找
****  

#### 分类书写
- 属性属于同一类的写一起
```css 
  // 推荐
  {
    font-size: 12px;
    font-family: 'Microsoft';
    padding: 4px;
    margin: 4px;
    width: 100px;
    height: 100px;
  }
  // 不推荐
  {
    font-size: 12px;
    font-family: 'Microsoft';
    padding: 4px;
    height: 100px;
    margin: 4px;
    width: 100px;
  }
```
</details>