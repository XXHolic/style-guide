# jsStandard
## h5 管理端维护规范
### 命名规范
#### 目录命名
- 全部采用小写方式，多个单词，用驼峰式，例如 `pageDetail`。

#### 文件命名
- 全部采用小写方式，多个单词，用驼峰式，例如 `pageDetail`。

### 目录规范
- page 下目录不能超过 3 层，包含 page 目录。
- 命名语义化。

### 原则
#### 块级别作用域用 const 或 let，尽量不要使用 var。
#### 声明引用
声明引用时，统一使用 const， 因为这样可以防止被修改，如果使用 var 可能会被修改导致问题很难排查。

#### 创建对象用字面量
```javascript
// bad
const item = new Object();
const arr = new Array();

// good
const item = {};
const arr = [];
```

#### Object
- 使用简写属性，简写属性和非简写属性分开写
```javascript
const name = 'Tom';
const age = '18';

// bad
const obj = {
  name: name,
  male: 'man',
  tall: 170,
  age: age,
};

// good
const obj = {
  name,
  age,
  male: 'man',
  tall: 170,
};
```
- 方法使用简写
```javascript
// bad
const atom = {
  value: 1,

  addValue: function (value) {
    return atom.value + value;
  },
};

// good
const atom = {
  value: 1,

  addValue(value) {
    return atom.value + value;
  },
};
```
- 合并或浅复制对象时，使用 `...` 替代 `Object.assign()` 。
```javascript
// very bad
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
delete copy.a; // so does this

// bad
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

// good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```
#### Array
- 复制数组使用 ...
```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i += 1) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```
- 将可遍历对象转换为一个数组，使用 ... 取代 Array.from
Array.from 方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。
```javascript
const foo = document.querySelectorAll('.foo');

// good
const nodes = Array.from(foo);

// best
const nodes = [...foo];
```
#### Function
1. 使用命名函数表达式取代函数声明，这样函数声明提升，可能在复杂的情况下不知道在什么产生了引用。
```javascript
// bad
function foo() {
  // ...
}

// bad
const foo = function () {
  // ...
};

// good
// lexical name distinguished from the variable-referenced invocation(s)
const short = function longUniqueMoreDescriptiveLexicalFoo() {
  // ...
};
```
2. 立即执行函数要用 () 包裹起来，不要在非函块中声明函数，例如 if、while。
3. 不要使用 arguments，或以这个单词命名参数。
```javascript
// bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// good
function concatenateAll(...args) {
  return args.join('');
}
```
4. 使用默认值语法取代变相赋值，有默认值的参数放到最后
```javascript
// really bad
function handleThings(name, opts) {
  // No! We shouldn’t mutate function arguments.
  // Double bad: if opts is falsy it'll be set to an object which may
  // be what you want but it can introduce subtle bugs.
  opts = opts || {};
  // ...
}

// still bad
function handleThings(name, opts) {
  if (opts === void 0) {
    opts = {};
  }
  // ...
}

// good
function handleThings(name, opts = {}) {
  // ...
}
```
#### 箭头函数
- 当你必须要使用匿名函数时，使用箭头函数，因为它包含了上下文的 this ，通常这是你所需要的。如果逻辑很复杂，就应该考虑单独抽出一个函数。
```javascript
// bad
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```
#### Class & Constructor
1. 使用 class 取代 prototype
```javascript
// bad
function Queue(contents = []) {
  this.queue = [...contents];
}
Queue.prototype.pop = function () {
  const value = this.queue[0];
  this.queue.splice(0, 1);
  return value;
};

// good
class Queue {
  constructor(contents = []) {
    this.queue = [...contents];
  }
  pop() {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  }
}
```
#### Iterator & Generator
-  不要使用 Iterator，推荐使用 高阶函数取代类似 `for-in` 或 `for-of` 的循环。
> Use map() / every() / filter() / find() / findIndex() / reduce() / some() / ... to iterate over arrays, and Object.keys() / Object.values() / Object.entries() to produce arrays so you can iterate over objects.
```javascript
const numbers = [1, 2, 3, 4, 5];

// bad
let sum = 0;
for (let num of numbers) {
  sum += num;
}
sum === 15;

// good
let sum = 0;
numbers.forEach((num) => {
  sum += num;
});
sum === 15;

// best (use the functional force)
const sum = numbers.reduce((total, num) => total + num, 0);
sum === 15;

// bad
const increasedByOne = [];
for (let i = 0; i < numbers.length; i++) {
  increasedByOne.push(numbers[i] + 1);
}

// good
const increasedByOne = [];
numbers.forEach((num) => {
  increasedByOne.push(num + 1);
});

// best (keeping it functional)
const increasedByOne = numbers.map(num => num + 1);
```
#### 尽量使用新的语法，例如解构。
#### 字符串统一使用单引号。
#### 不要使用 prototype。
#### 使用分号。
#### 禁止使用 with 语法和 eval()。
#### 使用全等比较
#### 永远不要直接使用undefined进行变量判断，使用typeof和字符串'undefined'对变量进行判断

### 代码风格

#### 缩进
使用 2 个空格。

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
以下几种情况不需要空格：

- 对象的属性名后
- 前缀一元运算符后
- 后缀一元运算符前
- 函数调用括号前
- 无论是函数声明还是函数表达式，'('前不要空格
- 数组的'['后和']'前
- 对象的'{'后和'}'前
- 运算符'('后和')'前

以下几种情况需要空格：

- 二元运算符前后
- 三元运算符'?:'前后
- 代码块'{'前
- 下列关键字前：else, while, catch, finally
- 下列关键字后：if, else, for, while, do, switch, case, try, catch, finally, with, return, typeof
- 单行注释'//'后（若单行注释和代码同行，则'//'前也需要），多行注释'*'后
- 对象的属性值前
- for循环，分号后留有一个空格，前置条件如果有多个，逗号后留一个空格
- 无论是函数声明还是函数表达式，'{'前一定要有空格
- 函数的参数之间

```javascript
// not good
var a = {
    b :1
};

// good
var a = {
    b: 1
};

// not good
++ x;
y ++;
z = x?1:2;

// good
++x;
y++;
z = x ? 1 : 2;

// not good
var a = [ 1, 2 ];

// good
var a = [1, 2];

// not good
var a = ( 1+2 )*3;

// good
var a = (1 + 2) * 3;

// no space before '(', one space before '{', one space between function parameters
var doSomething = function(a, b, c) {
    // do something
};

// no space before '('
doSomething(item);

// not good
for(i=0;i<6;i++){
    x++;
}

// good
for (i = 0; i < 6; i++) {
    x++;
}
```

#### 空行
以下几种情况需要空行：

- 变量声明后（当变量声明在代码块的最后一行时，则无需空行）
- 注释前（当注释在代码块的第一行时，则无需空行）
- 代码块后（在函数调用、数组、对象中则无需空行）
- 文件最后保留一个空行
```javascript
// need blank line after variable declaration
var x = 1;

// not need blank line when variable declaration is last expression in the current block
if (x >= 1) {
    var y = x + 1;
}

var a = 2;

// need blank line before line comment
a++;

function b() {
    // not need blank line when comment is first line of block
    return a;
}

// need blank line after blocks
for (var i = 0; i < 2; i++) {
    if (true) {
        return false;
    }

    continue;
}

// not good
var obj = {
    foo: function() {
        return 1;
    },

    bar: function() {
        return 2;
    }
};

// not need blank line when in argument list, array, object
func(
    2,
    function() {
        a++;
    },
    3
);

var foo = [
    2,
    function() {
        a++;
    },
    3
];


var foo = {
    a: 2,
    b: function() {
        a++;
    },
    c: 3
};
```
#### 换行
换行的地方，行末必须有 `,` 或者运算符 `；`

以下几种情况不需要换行：

- 下列关键字后：else, catch, finally
- 代码块'{'前
以下几种情况需要换行：

- 代码块'{'后和'}'前
- 变量赋值后
```javascript
// not good
var a = {
    b: 1
    , c: 2
};

x = y
    ? 1 : 2;

// good
var a = {
    b: 1,
    c: 2
};

x = y ? 1 : 2;
x = y ?
    1 : 2;

// no need line break with 'else', 'catch', 'finally'
if (condition) {
    ...
} else {
    ...
}

try {
    ...
} catch (e) {
    ...
} finally {
    ...
}

// not good
function test()
{
    ...
}

// good
function test() {
    ...
}

// not good
var a, foo = 7, b,
    c, bar = 8;

// good
var a,
    foo = 7,
    b, c, bar = 8;
```
#### 注释
- 单行注释单独一行
- 多上注释最少三行
```javascript

// one space after code
var name = 'Tom';

/*
 * one space after '*'
 */
var x = 1;

```

#### 引号

#### 括号
下列关键字后必须有大括号（即使代码块的内容只有一行）：
- if,
- else,
- for,
- while,
- do,
- switch,
- try,
- catch,
- finally,
- with

#### 变量
- 标准变量采用驼峰式命名（除了对象的属性外，主要是考虑到cgi返回的数据）
- 'ID'在变量名中全大写
- 'URL'在变量名中全大写
- 'Android'在变量名中大写第一个字母
- 'IOS'全部大写
- 常量全大写，用下划线连接

#### 对象
- 属性名不要加引号，除非出现无法解析的情况，例如 name-data 属性。

#### 数组

#### 函数
归属到了原则

#### 字符串
归属到原则

#### 原型
- 尽量使用 class 取代 prototype

#### 模块
归属到原则

#### 迭代器
归属到原则