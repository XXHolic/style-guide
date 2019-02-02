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