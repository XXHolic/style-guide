# jsStandard
## h5 管理端维护规范
### 命名规范

- 文件名采用 `驼峰`
- 工具类采用 `utils` 命名

****

### 原则

- 临时处理的代码需要注释，以便及时删掉。例：生鲜分拣损耗统计的小红点功能

****

### 代码风格

#### 缩进

- 2个字符

****

#### 分号

- 需要带分号的
  - 执行的函数后
  - 变量声明
  - 暴漏出去的组件/函数后面。export Component `;`

- 不需要分号的
  - 组件内的函数声明

****  

#### 空格

- 带空格
  - `{` 前
  - `=` 前后
  - `:` 后

****

#### 空行

- `}` 后
- `return` 前
- 变量与执行函数之间
  ```javascript
  const a = 4;

  this.b()
  ```

****

#### 换行

- `props` 大于等于2个时
- `解构取值` 达到5个时

****

#### 注释

- 组件说明/工具类的函数说明用 `块注释`
- 简短地用 `行注释`

****

#### 引号

- `''`

****

#### 括号



#### 变量

- `驼峰命名`
- 过长的单词能简写就简写

****

#### 对象

- 采取声明式创建，不采取 `new Object()` 的形式,因为参数的检测方式过于宽松,无法预期返回值。
  ```javascript
    new Object(0) => Number{0}
    new Object('0') => String{'0'}
  ```
  此时得到的是一个包装类对象

- 属性统一写在 `{}`里
  ```javascript
    // bad                 //good
    let obj = {};          let obj = {a: 4}
    obj.a = 4 
  ```   

#### 数组

- 声明式创建

****

#### 函数

- 需要统一绑定 `this` 的方式。
  - 目前项目里有用 `functionName = () => {}` 绑定 `this` 的。
  - `autobind` 绑定 `this` 的
  - `autobind-decorator` 提供了三种绑定 `this` 的方法，分别是 `boundClass`、 `boundMethod`、 `autobind`

- 工具类的使用 `es5` 方式声明 

****

#### 字符串

- 直接用 `''` 声明

****

#### 原型

- 为某个方法添加原型上的方法时，不建议直接重写prototype对象,在继承时可能会出现问题，constructor指针不对，如果这样的话，需要在prototype手动添加constructor
  ```javascript
    // bad
    function fn () {}
    fn.prototype = {

    }

    // good 
    fn.prototype.a = function () {}

    // or 
    fn.prototype = {
      constructor: xxx
    }

  ```

#### 模块

- 按照功能命名与文件夹/文件名一致

#### 迭代器