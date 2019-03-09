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

### 代码风格

#### 缩进

#### 分号

#### 空格

#### 空行

#### 换行

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