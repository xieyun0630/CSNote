我们使用浏览器作为工作环境，所以基本的 UI 功能将是：

-  {{c1::  prompt(question[, default])}}

  询问一个问题，并返回访问者输入的内容，如果他按下「取消」则返回 `null`。

-  {{c1::  confirm(question)}}

  提出一个问题，并建议在确定和取消之间进行选择。该选项以 `true/false` 形式返回。

-  {{c1::  alert(message)}}

  输出一个 `消息`。

所有这些函数都会产生**模态框**，它们会暂停代码执行并阻止访问者与页面交互，直到用户输入内容。

### 使用幂运算符  [	](javascript_info_20191219101334390)

 {{c1::  

```javascript
alert( 2 ** 2 ); // 4  (2 * 2)
alert( 2 ** 3 ); // 8  (2 * 2 * 2) 
alert( 2 ** 4 ); // 16 (2 * 2 * 2 * 2)
```

}}

### 函数表达式 vs 函数声明  [	](javascript_info_20191219101334391)

 {{c1::  

**严格模式下，当一个函数声明在一个代码块内运行时，它在该块内的任何地方都是可见的。但块外则相反。**

函数表达式需要执行到以后才会生效。

```js
sayHi("John"); // error!
let sayHi = function(name) {  // (*) no magic any more
  alert( `Hello, ${name}` );
};
```

}}

### `typeof` 运算符返回值的类型，但有两个例外：  [	](javascript_info_20191219101334394)

 {{c1::  

```javascript
typeof null == "object" // 语言的设计错误
typeof function(){} == "function" // 函数被特殊对待
```

引用：[变量](https://zh.javascript.info/variables) 和 [数据类型](https://zh.javascript.info/types)。

}}

### “switch” 结构  [	](javascript_info_20191219101334395)

“switch” 结构可以替代多个 `if` 检查，它内部使用 {{c1::   `===`}}（严格相等）进行比较。

例如：

```javascript
let age = prompt('Your age?', 18);

switch (age) {
  case 18:
    alert("Won't work");

  case "18":
    alert("This works!");
    break;

  default:
    alert("Any value not equal to one above");
}
```

 //{{c1::   提示的结果是一个字符串，而不是数字}}

### Debugger 命令 [>](https://zh.javascript.info/debugging-chrome#debugger-ming-ling)  [	](javascript_info_20191219101334397)

{{c1::  

我们也可以使用 `debugger` 命令来暂停代码，像这样：

```javascript
function hello(name) {
  let phrase = `Hello, ${name}!`;

  debugger;  // <-- 调试器会在这停止

  say(phrase);
}
```

当我们在一个代码编辑器中并且不想切换到浏览器在开发者工具中查找脚本来设置断点时，这真的是非常方便啦。

}}

## Objects（对象）：基础知识 [》](https://zh.javascript.info/object-basics)  [	](javascript_info_20191219101334399)

### 可以用下面两种语法的任一种来创建一个空的对象（“空柜子”）：  [	](javascript_info_20191219101334400)

{{c1::  

```javascript
let user = new Object(); // “构造函数” 的语法
let user = {};  // “字面量” 的语法
```

}}

### 可以用多字词语来作为属性名：  [	](javascript_info_20191219101334402)

{{c1::  

```javascript
let user = {
  name: "John",
  age: 30,
  "likes birds": true  // 多词属性名必须加引号
};
```

```javascript
// 语法错误
user.likes birds = true
```

```javascript
// set
user["likes birds"] = true;

// get
alert(user["likes birds"]); // true
```

}}

### 什么是计算属性？  [	](javascript_info_20191219101334404)

{{c1::  

```javascript
let fruit = prompt("Which fruit to buy?", "apple");

let bag = {
  [fruit]: 5, // 属性名从 fruit 变量中计算
};

alert( bag.apple ); // 5 如果 fruit="apple"
```

}}

### 用存在的变量当做属性名时的简写  [	](javascript_info_20191219101334407)
{{c1::  
我们可以把简写方式和正常方式混用：

```javascript
let user = {
  name,  // 与 name:name 相同
  age: 30
};
```
}}

### 对象的存在值检查  [	](javascript_info_20191219101334408)

```javascript
let user = {};
//{{c1::  
alert( user.noSuchProperty === undefined ); }}// true 意思是没有这个属性

//同样也有一个特别的操作符 "in" 来检查是否属性存在。
//{{c1::  
"key" in object 
//注意 in 的左边必须是属性名。通常是一个字符串，如果不用字符串，那就是一个字符串变量。}}
```

### 对象属性的顺序？  [	](javascript_info_20191219101334411)

{{c1::  

”有特别的顺序“：整数属性有顺序，其他的是按照创建的顺序排序。}}

### 整数属性是什么？  [	](javascript_info_20191219101334412)

{{c1::  

```javascript
//“49” 是一个整数属性名，因为我们把它转换成整数，再转换回来，它还是一样。但是 “+49” 和 “1.2” 就不行了：
// Math.trunc 是内置的去除小数部分的方法。
alert( String(Math.trunc(Number("49"))) ); // "49"，同样，整数属性
alert( String(Math.trunc(Number("+49"))) ); // "49"，不同于 "+49" ⇒ 不是整数属性
alert( String(Math.trunc(Number("1.2"))) ); // "1"，不同于 "1.2" ⇒ 不是整数属性
```

}}

### ` Object.assign()`的使用  [	](javascript_info_20191219101334415)

```javascript
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// 把 permissions1 和 permissions2 的所有属性都拷贝给 user
//{{c1::  
Object.assign(user, permissions1, permissions2);
 }}
// 现在 user = { name: "John", canView: true, canEdit: true }
// 如果接收的对象（user）已经有了同样属性名的属性，{{c1:: 前面的会被覆盖}}
```



### 按下面的要求写代码：  [	](javascript_info_20191219101334421)

1. 创建一个空的 `user` 对象.
2. 为这个对象增加一个属性，键是 `name` 值是 `John`。
3. 再增加一个属性键`surname`值 `Smith`。
4. 把 `name` 属性的值改成 `Pete`。
5. 从对象中删除 `name` 属性

---
{{c1::  
```js
let user={};
user.name="John";
user.surname="Smith";
user.name="pete";
delete user.name;
```
}}

### Symbol类型  [	](javascript_info_20191219101334424)

Symbol 总是不同的值，即使它们有相同的名称。如果我们希望同名 Symbol 相等，那么{{c1::  我们应该使用全局注册表：`Symbol.for(key)` 返回（如果需要的话创建）一个以 `key` 作为名称的全局 Symbol。`Symbol.for` 的多次调用完全返回相同的 Symbol。}}

Symbol 不是 100% 隐藏的。有一个内置方法 {{c1::  [Object.getOwnPropertySymbols(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) }}允许我们获取所有的 Symbol。还有一个名为{{c1::   [Reflect.ownKeys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) }}返回**所有**键，包括 Symbol。

### 对象方法与 "this"  [	](javascript_info_20191219101334426)

this总是指向{{c1::  该方法所属的对象。}}

非严格模式下，函数中(没有定义在对象中的函数)的`this` 将会是{{c1::  **全局对象**（浏览器中的 `window`）。}}

### 箭头函数没有自己的 “this”  [	](javascript_info_20191219101334428)

{{c1::  

箭头函数的this指向当前函数内的this。

```javascript
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};
//this 值取决于外部『正常的』函数
user.sayHi(); // Ilya
```

}}

### 在对象字面量中使用 "this"  [	](javascript_info_20191219101334432)

这里 `makeUser` 函数返回了一个对象。

访问 `ref` 的结果是什么？为什么？

```javascript
function makeUser() {
  return {
    name: "John",
    ref: this
  };
};

let user = makeUser();

alert( user.ref.name ); // What's the result?
```

---

{{c1::  
1. 这里的this实际上取得的是window对象，也就是说user.ref.name被看作成window的属性。
2. 严格模式下的这里的 `this` 值为 `undefined`
3. 在方法中的this返回的是该方法所属的对象。
}}

### `Symbol.toPrimitive` 对象原始值转换的使用？  [	](javascript_info_20191219101334436)

```javascript
//第一种方式
let obj={};
//{{c1::  
obj[Symbol.toPrimitive] = function(hint) {
  // 返回一个原始值
  // hint = "string"，"number" 和 "default" 中的一个
}
}}

//第二种方式
//{{c1::  
let user = {
  name: "John",
  money: 1000,

  [Symbol.toPrimitive](hint) {
    alert(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
};
}}

// 转换演示：
alert(user); // hint: string -> {name: "John"}
alert(+user); // hint: number -> 1000
alert(user + 500); // hint: default -> 1500
```

### 为了进行转换，JavaScript 尝试查找并调用三个对象方法：  [	](javascript_info_20191219101334438)

{{c1::  

1. 调用 `obj[Symbol.toPrimitive](hint)` 如果这个方法存在的话，

2. 否则如果暗示是"string"
   - 尝试 `obj.toString()` 和 `obj.valueOf()`，无论哪个存在。
3. 否则，如果暗示"number"或者"default"
   - 尝试 `obj.valueOf()` 和 `obj.toString()`，无论哪个存在。

}}

### 对象中方法定义的简写  [	](javascript_info_20191219101334439)

{{c1::  

```javascript
// 方法简写看起来更好，对吧？
let user = {
  sayHi() { // 与 "sayHi: function()" 一样
    alert("Hello");
  }
};
```

}}

### 使用this的好处  [	](javascript_info_20191219101334440)

{{c1::  

```javascript
let user = {
  name: "John",
  age: 30,
  sayHi() {
    alert( user.name ); // 导致错误
  }
};

let admin = user;
user = null; // 覆盖让其更易懂

admin.sayHi(); // 噢哟！在 sayHi() 使用了旧的变量名。错误！
```

}}

### 解释 "this" 的值  [	](javascript_info_20191219101334441)

在下面的代码中，我们试图连续调用 4 次 `user.go()` 方法。

但是 `(1)` 和 `(2)` 次 `(3)` 和 `(4)` 调用结果不同，为什么呢？

```javascript
let obj, method;

obj = {
  go: function() { alert(this); }
};

obj.go();               // (1) [object Object]

(obj.go)();             // (2) [object Object]

(method = obj.go)();    // (3) undefined

(obj.go || obj.stop)(); // (4) undefined
```

---

这里是解释。

{{c1::  

1. 它是一个常规的方法调用。

2. 同样，括号没有改变执行的顺序，点总是首先执行。

3. 这里我们有一个更复杂的 `(expression).method()` 调用。这个调用就像被分成了两行（代码）一样：

   ```javascript
   f = obj.go; // calculate the expression
   f();        // call what we have
   ```

这里的 `f()` 是作为一个没有（设定）`this` 的函数执行的。

1. 与 `(3)` 相类似，在点 `.` 的左边也有一个表达式。

要解释 `(3)` 和 `(4)` 的原因，我们需要回顾一下属性访问器（点或方括号）返回的值是引用类型的。

}}

{{c1::  

除了方法调用之外的任何操作（如赋值 `=` 或 `||` 等）把它变为了一个没有设定 `this` 信息的普通值。

}}

## 构造函数  [	](javascript_info_20191219101334443)

### 构造函数概念  [	](javascript_info_20191219101334445)

构造函数在技术上是常规函数。不过有两个约定：

{{c1::  

1. 他们首先用大写字母命名。
2. 它们只能用 `new` 操作符来执行。

}}

### 当一个函数作为 `new User(...)`执行时，它执行以下步骤  [	](javascript_info_20191219101334447)

{{c1::  

1. 一个新的空对象被创建并分配给 `this`。
2. 函数体执行。通常它会修改 `this`，为其添加新的属性。
3. 返回 `this` 的值。

}}

从技术上讲，**任何函数都可以用作构造函数**。即：任何函数都可以运行 `new`，它会执行上面的算法。

如果没有参数我们 `new` 可以省略括号：

{{c1::  

```javascript
let user = new User; // <-- no parentheses
// same as
let user = new User();
//这里省略括号不被认为是一种“好风格”，但是规范允许使用该语法。
```

}}

### 双语法构造函数：`new.target`的含义?  [	](javascript_info_20191219101334449)

 {{c1::   

**`new.target`**属性允许你检测函数或构造方法是否是通过[new](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)运算符被调用的。在通过[new](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)运算符被初始化的函数或构造方法中，`new.target`返回一个指向构造方法或函数的引用。在普通的函数调用中，`new.target` 的值是[`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。

```javascript
function User() {
  alert(new.target);
}

// 不带 new：
User(); // undefined

// 带 new：
new User(); // function User { ... }
```

}}

### task: 构造函数 Return返回的是什么?  [	](javascript_info_20191219101334450)

是否可以创建函数 `A` 和 `B`，如 `new A()==new B()`？

```javascript
function A() { ... }
function B() { ... }

let a = new A;
let b = new B;

alert( a == b ); // true
```

如果可以，请提供他们的代码示例?

 {{c1::   

**带有对象的 `return` 返回该对象**，在所有其他情况下返回 `this`。

```javascript
let obj = {};

function A() { return obj; }
function B() { return obj; }

alert( new A() == new B() ); // true
```

}}

## 数据类型  [	](javascript_info_20191219101334452)

### 在 JavaScript 中有 6 种基本类型：  [	](javascript_info_20191219101334454)

 {{c1::   

- `string`、`number`、`boolean`、`symbol`、`null` 和 `undefined`。

  }}

### 基本类型的包装对象  [	](javascript_info_20191219101334455)

极其不推荐使用构造函数 `String/Number/Boolean`，为什么？

特殊的基本类型 {{c1::    `null` 和 `undefined` }}是个例外。他们没有相应的“包装对象”，

基本类型不是{{c1::    对象。}}

基本类型{{c1:: 不能存储数据。}}

所有的属性/方法操作都是在{{c1:: 临时对象}}的帮助下执行的。

---

```javascript
alert( typeof 1 ); // "number"
alert( typeof new Number(1) ); // "object"!
//导致下面问题
let zero = new Number(0);
if (zero) { // zero is true, because it's an object
  alert( "zero is truthy?!?" );
}
```

## 数字类型  [	](javascript_info_20191219101334457)

### `js`中编写数字的更多方法  [	](javascript_info_20191219101334459)

```js
//数字简写
1e-3 =//{{c1:: 1 / 1000 (=0.001)}}
1.23e-6 =//{{c1:: 1.23 / 1000000 (=0.00000123) }}
//16进制
//{{c1::
alert( 0xff ); // 255
alert( 0xFF ); // 255 (the same, case doesn't matter)
 }}
//2进制
//{{c1::
let a = 0b11111111; // binary form of 255
}}
////8进制
//{{c1::
let b = 0o377; // octal form of 255
 }}
alert( a == b ); // true, the same number 255 at both sides
```

### `num.toString(base)`  [	](javascript_info_20191219101334460)

{{c1::

```javascript
let num = 255;

alert( num.toString(16) );  // ff
alert( num.toString(2) );   // 11111111
```

常见的用例如下：

- **base=16** 用于十六进制颜色，字符编码等，数字可以是 `0..9` 或 `A..F`。

- **base=2** 主要用于调试按位操作，数字可以是 `0` 或 `1`。

- **base=36** 是最大值，数字可以是 `0..9` 或 `A..Z`。整个拉丁字母用来表示一个数字。对于 `36` 来说，一个有趣而有用的例子是，当我们需要将一个较长的数字标识符变成较短的时候，例如做一个简短的URL。可以简单地用基数为 `36` 的数字系统表示：

  }}

### 数字调用方法的两个点  [	](javascript_info_20191219101334462)

```javascript
alert( 123456..toString(36) ); // 2n9c
```

---

{{c1::请注意 `123456..toString(36)` 中的两个点不是拼写错误。如果我们想直接在一个数字上调用一个方法，比如上面例子中的 `toString`，那么我们需要在它后面放置两个点 `..`。

如果我们放置一个点：`123456.toString(36)`，那么会出现错误，因为 JavaScript 语法暗示了第一个点之后的小数部分。如果我们再放一个点，那么 JavaScript 知道小数部分是空的，现在进入方法。

也可以写 `(123456).toString(36)`。}}

### `Math.trunc`方法  [	](javascript_info_20191219101334464)

{{c1::

删除小数点后的所有内容而不舍入：`3.1` 变成 `3`，`-1.1` 变成 `-1`。}}

### ` num.toFixed(n)`方法  [	](javascript_info_20191219101334465)

{{c1::函数 [toFixed(n)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) 将点数后的数字**四舍五入到 `n` 个数字**并返回结果的**字符串**表示。}}

```javascript
let num = 12.34;
alert( num.toFixed(1) ); {{c1::// "12.3"}}

let num = 12.36;
alert( num.toFixed(1) ); {{c1::// "12.4"}}
```

### `js`中`isNaN`和`Number.isNaN`的区别  [	](javascript_info_20191219101334468)

{{c1::

`Number.isNaN`与`isNaN`最的区别是，`Number.isNaN` 不存在类型转换的行为。

```java
console.log(isNaN('测试')) //true 因为字符串是有可能转换成数字的
console.log(Number.isNaN('测试')) //false
alert( NaN === NaN ); // false
```

}}

### 测试：`isFinite`  [	](javascript_info_20191219101334470)

- `isFinite(value)`: {{c1::将其参数转换为数字，如果是常规数字，则返回 `true`，而不是 `NaN / Infinity / -Infinity`：

  ```javascript
  alert( isFinite("15") ); // true
  alert( isFinite("str") ); // false, because a special value: NaN
  alert( isFinite(Infinity) ); // false, because a special value: Infinity
  ```

  }}

- `
  Object.is`可以比较 `===` 等值，但对于两种边缘情况更可靠：{{c1::
  
  1. 它适用于 `NaN`： `Object.is（NaN，NaN）=== true`，这是件好事。
  2. 值 `0` 和 `-0` 是不同的：`Object.is（0，-0）=== false`，它不是很重要，但这些值在技术上是不同的。}}

### `parseInt` 和` parseFloat`  [	](javascript_info_20191219101334472)

{{c1::

使用加号 `+` 或 `Number()` 的数字转换是严格的。如果一个值不完全是一个数字，就会失败：

```javascript
alert( +"100px" ); // NaN
```

 `parseInt` 和 `parseFloat` 从字符串中“读出”一个数字，直到他们可以。如果发生错误，则返回收集的数字:

- `parseInt(str，base)` 解析来自任何数字系统的整数，其基数为：`2≤base≤36`。

```javascript
alert( parseInt('100px') ); // 100
alert( parseFloat('12.5em') ); // 12.5
alert( parseInt('12.3') ); // 12, only the integer part is returned
alert( parseFloat('12.3.4') ); // 12.3, the second point stops the reading

alert( parseInt('a123') ); // NaN, the first symbol stops the process
```

}}

## 字符串  [	](javascript_info_20191219101334473)

### 3种字符串类型：  [	](javascript_info_20191219101334474)

{{c1::

在 JavaScript 中，字符串不可更改。改变字符是不可能的。

字符串可以包含在单引号、双引号或反引号中：

```javascript
let single = 'single-quoted';
let double = "double-quoted";
let backticks = `backticks`;
```

}}

### 可以使用 `for..of` 遍历字符：  [	](javascript_info_20191219101334476)

{{c1::

```javascript
for (let char of "Hello") {
  alert(char); // H,e,l,l,o （char 变为“H”，然后是“e”，然后是“l”等）
}
```

}}

### 按位（bitwise）NOT 使用`indexOf`的技巧  [	](javascript_info_20191219101334477)

{{c1::

```javascript
let str = "Widget";

if (~str.indexOf("Widget")) {
  alert( 'Found it!' ); // 正常运行
}
```

}}

### `str.includes(substr, pos)`  [	](javascript_info_20191219101334479)

{{c1::

更现代的方法 [str.includes(substr, pos)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/includes) 取决于 `str` 是否包含 `substr` 来返回 `true/false`。

```javascript
alert( "Widget with id".includes("Widget") ); // true
alert( "Hello".includes("Bye") ); // false
alert( "Midget".includes("id") ); // true
alert( "Midget".includes("id", 3) ); // false, 位置 3 没有“id”
```

}}

### JavaScript 中有三种获取字符串的方法：`substring`、`substr` 和 `slice`。  [	](javascript_info_20191219101334481)

| 方法                    | 选择方式……                                         | 负号参数            |
| :---------------------- | -------------------------------------------------- | :------------------ |
| `slice(start, end)`     | **{{c1:: 从** `start` 到 `end` (不含 `end`)}}      | 允许                |
| `substring(start, end)` | `{{c1:: start` 与 `end` 之间}}                     | 负值代表 `0`        |
| `substr(start, length)` | {{c1:: 从 `start` 开始获取长为 `length` 的字符串}} | 允许 `start` 为负数 |

## 数组  [	](javascript_info_20191219101334483)

### 创建一个空数组有两种语法：  [	](javascript_info_20191219101334484)

{{c1::

```javascript
let arr = new Array();
let arr = [];
```

}}

### 数组可以存储任何类型的元素。  [	](javascript_info_20191219101334485)

- 数组和对象一样， 都可以在末尾{{c1::冗余一个逗号。}}
- 数组是一种特殊的对象。{{c1:: 使用方括号来访问属性 `arr[0]` 实际上是来自于对象的语法。这个数字被用作键值。他们扩展了对象，提供了特殊的方法来处理有序的数据集合，还添加了 `length` 属性。但是核心还是一个对象。}}
- 数组有自己的 `toString` 方法的实现，会返回以逗号隔开的元素列表。

```javascript
// 混合值
let arr = [ 'Apple', { name: 'John' }, true, function() { alert('hello'); } ];

// 获取索引为 1 的对象然后显示它的 name
alert( arr[1].name ); // John

// 获取索引为 3 的函数并执行
arr[3](); // hello
```

### pop/push, shift/unshift 方法  [	](javascript_info_20191219101334487)

- `push` {{c1:: 在末端添加一个元素。}}
- `shift` {{c1:: 取出队列最前端的一个元素，整个队列往前移，这样原先排第二的元素现在排在了第一。}}
- `pop` {{c1:: 从末端取出一个元素。}}
- `unshift` {{c1:: 在数组的前端添加元素。![1571887679989](javascript_info.assets/1571887679989.png)
}}

### 数组误用的几种方式:  [	](javascript_info_20191219101334489)

- 添加一个非数字的属性比如    `arr.test = 5`。}}
- 制造空洞，比如：{{c1::  添加 `arr[0]` 后添加 `arr[1000]` (它们中间什么都没有)。}}
- 以倒序填充数组, {{c1::  比如 `arr[1000]`，`arr[999]` 等等。}}

### 2种简化for循环的格式区别：  [	](javascript_info_20191219101334491)

- `for..in`： {{c1::  遍历对象的属性名。}}
- `for..of `： {{c1::  遍历数据元素。}}

### `length` 代表数组的大小 [	](javascript_info_20191219101334492)

`length` 属性的特点是：

-  {{c1::它是可写的。}}
- {{c1::且自动更新。}}

所以，清空数组最好的方法就是： {{c1::  ``arr.length = 0;``   }}。

## 数组方法  [	](javascript_info_20191219101334493)

### `arr.splice(index[, deleteCount, elem1, ..., elemN])`  [	](javascript_info_20191219101334495)

 {{c1:: 从 `index` 开始：删除 `deleteCount` 元素并在当前位置插入 `elem1, ..., elemN`。最后返回已删除元素的数组。 }}

**允许负向索引**

### `arr.slice(start, end)`  [	](javascript_info_20191219101334496)

  {{c1:: 它从所有元素的开始索引 `"start"` 复制到 `"end"` (不包括 `"end"`) 返回一个新的数组。 }} 

### `arr.concat(arg1, arg2...)`  [	](javascript_info_20191219101334497)

结果是一个{{c1:: 包含`arr`，`arg1`，`arg2`等元素的新数组。}}

如果参数是一个数组或具有 `Symbol.isConcatSpreadable` 属性， {{c1:: 则其所有元素都将被复制。否则，复制参数本身。

```javascript
let arr = [1, 2];
let arrayLike = {
  0: "something",
  length: 1
};
alert( arr.concat(arrayLike) ); // 1,2,[object Object]
//[1, 2, arrayLike]

let arr = [1, 2];

let arrayLike = {
  0: "something",
  1: "else",
  [Symbol.isConcatSpreadable]: true,
  length: 2
};

alert( arr.concat(arrayLike) ); // 1,2,something,else
```

 }}

### `indexOf` `lastIndexOf ` `includes`  [	](javascript_info_20191219101334499)

- `arr.indexOf(item, from)` {{c1:: 从索引 `from` 查询 `item`，如果找到返回索引，否则返回 `-1`。}}
- `arr.lastIndexOf(item, from)` — {{c1:: 和上面相同，只是从尾部开始查询。}}
- `arr.includes(item, from)` — {{c1:: 从索引 `from` 查询 `item`，如果找到则返回 `true`。}}

### JS数组中使用lambda表达式的方法  [	](javascript_info_20191219101334501)

| [filter()](https://www.runoob.com/jsref/jsref-filter.html)   | {{c1::检测数值元素，并返回符合条件所有元素的数组。}}         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [find()](https://www.runoob.com/jsref/jsref-find.html)       | {{c1::返回符合传入测试（函数）条件的数组元素。}}             |
| [findIndex()](https://www.runoob.com/jsref/jsref-findindex.html) | {{c1::返回符合传入测试（函数）条件的数组元素索引。}}         |
| [forEach()](https://www.runoob.com/jsref/jsref-foreach.html) | {{c1::数组每个元素都执行一次回调函数。}}                     |
| [map()](https://www.runoob.com/jsref/jsref-map.html)         | {{c1::通过指定函数处理数组的每个元素，并返回处理后的数组。}} |
``` js
//参数函数
//{{c1:: 
    let func=function(item, index, array) {
      // 如果查询到返回 true
    };
}}
    let result = arr.find(func);

//箭头函数版
//{{c1:: 
    let someUsers = users.filter(item => item.id < 3);
}}
    alert(someUsers.length); // 2
```

- {{c1:: `item` 是元素。
- `index` 是它的索引。
- `array` 是数组本身。}}


### `arr.sort(fn)`  [	](javascript_info_20191219101334502)

**默认情况下按字符串排序。**

自定义排序例子：

  {{c1::  

```javascript
function compareNumeric(a, b) {
  if (a > b) return 1;
  if (a == b) return 0;
  if (a < b) return -1;
}

let arr = [ 1, 2, 15 ];

arr.sort(compareNumeric);

alert(arr);  // 1, 2, 15
```

}}

### `arr.reverse`  [	](javascript_info_20191219101334504)

  {{c1::  

[arr.reverse](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) 方法颠倒 `arr` 中元素的顺序。

}}

### `str.split `和`arr.join`  [	](javascript_info_20191219101334506)

`str.split `:  {{c1::  分割字符串为一个数组。}}

`arr.join`:  {{c1::  将数据拼接成字符串}}

### `arr.reduce()`与`reduceRight()`  [	](javascript_info_20191219101334508)

| [reduce()](https://www.runoob.com/jsref/jsref-reduce.html)   | {{c1::将数组元素计算为一个值（从左到右）。}} |
| ------------------------------------------------------------ | :------------------------------------------- |
| [reduceRight()](https://www.runoob.com/jsref/jsref-reduceright.html) | {{c1::将数组元素计算为一个值（从右到左）。}} |

**参数列表示例**：
 {{c1::  

```js
let value = arr.reduce(function(previousValue, item, index, arr) {
  // ...
}, initial);
```
}}

### `Array.isArray` 的作用  [	](javascript_info_20191219101334509)

 {{c1::  

数组基于对象。不构成单独的语言类型。

所以 `typeof` 无法从对象中区分出数组来：

```javascript
alert(Array.isArray({})); // false

alert(Array.isArray([])); // true
```

}}

### `thisArg`的含义  [	](javascript_info_20191219101334511)

thisArg为当前函数的运行环境

```javascript
let user = {
  age: 18,
  younger(otherUser) {
    return otherUser.age < this.age;
  }
};

let users = [
  {age: 12},
  {age: 16},
  {age: 32}
];

// 找到比 user 小的所有 users
let youngerUsers = users.filter(user.younger, user);

alert(youngerUsers.length); // 2
```

 {{c1::  

在上面我们使用 `user.younger` 作为过滤器，并提供 `user` 作为它的上下文。如果我们没有提供上下文，`users.filter(user.younger)` 会调用`user.younger` 作为一个独立的函数，这时 `this=undefined`。

}}

### 数组方法总结  [	](javascript_info_20191219101334512)

- 添加/删除元素：
  - `push(...items)` — 从结尾添加元素，
  - `pop()` — 从结尾提取元素，
  - `shift()` — 从开头提取元素，
  - `unshift(...items)` — 从开头添加元素，
  - `splice(pos, deleteCount, ...items)` — 从 `index` 开始：删除 `deleteCount` 元素并在当前位置插入元素。
  - `slice(start, end)` — 它从所有元素的开始索引 `"start"` 复制到 `"end"` (不包括 `"end"`) 返回一个新的数组。
  - `concat(...items)` — 返回一个新数组：复制当前数组的所有成员并向其中添加 `items`。如果有任何`items` 是一个数组，那么就取其元素。
- 查询元素：
  - `indexOf/lastIndexOf(item, pos)` — 从 `pos` 找到 `item`，则返回索引否则返回 `-1`。
  - `includes(value)` — 如果数组有 `value`，则返回 `true`，否则返回 `false`。
  - `find/filter(func)` — 通过函数过滤元素，返回 `true` 条件的符合 find 函数的第一个值或符合 filter 函数的全部值。
  - `findIndex` 和 `find` 类似，但返回索引而不是值。
- 转换数组：
  - `map(func)` — 从每个元素调用 `func` 的结果创建一个新数组。
  - `sort(func)` — 将数组倒序排列，然后返回。
  - `reverse()` — 在原地颠倒数组，然后返回它。
  - `split/join` — 将字符串转换为数组并返回。
  - `reduce(func, initial)` — 通过为每个元素调用 `func` 计算数组上的单个值并在调用之间传递中间结果。
- 迭代元素：
  - `forEach(func)` — 为每个元素调用 `func`，不返回任何东西。
- 其他：  – `Array.isArray(arr)` 检查 `arr` 是否是一个数组。

请注意，`sort`，`reverse` 和 `splice` 方法修改数组本身。

这些方法是最常用的方法，它们覆盖 99％ 的用例。但是还有其他几个：

- [arr.some(fn)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/some)/[arr.every(fn)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/every) 检查数组。

  在类似于 `map` 的数组的每个元素上调用函数 `fn`。如果任何/所有结果为 `true`，则返回 `true`，否则返回 `false`。

- [arr.fill(value, start, end)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/fill) — 从 `start` 到 `end` 用 `value` 重复填充数组。

- [arr.copyWithin(target, start, end)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin) —将其元素从 `start` 到 `end` 在 `target` 位置复制到 **本身**（覆盖现有）。

## `Symbol.iterator`（可迭代对象）  [	](javascript_info_20191219101334513)

### 可迭代对象概念  [	](javascript_info_20191219101334515)

- `obj[Symbol.iterator]` 方法返回的结果被称为   {{c1::   **迭代器**}}。由它处理更深入的迭代过程。
- 一个迭代器必须有    {{c1::   `next()` 方法}}，它返回一个    {{c1::   `{done: Boolean, value: any}`，这里 `done:true` 表明迭代结束，否则 `value` 就是下一个值。}}

### 实现`Symbol.iterator` 的两种方式  [	](javascript_info_20191219101334516)

- 第一种：{{c1::每次调用`obj[Symbol.iterator]`都返回一个实现了next()方法的新对象。}}
  - 特点：{{c1::每次会返回新的迭代器从头开始迭代。}}
- 第二种:   每次调用`obj[Symbol.iterator]`都返回`this`,对象本身实现了next方法。
  - 特点：{{c1::对象内部会记住迭代的状态。}}

### 显式调用字符串的迭代器  [	](javascript_info_20191219101334518)

```javascript
let str = "Hello";
// 要和下面代码完成的功能一致
// for (let char of str) alert(char);
//{{c1:: 
let iterator = str[Symbol.iterator]();
while (true) {
  let result = iterator.next();
  if (result.done) break;
  alert(result.value); // 一个一个输出字符
}
}}
```

### 类数组对象  [	](javascript_info_20191219101334520)

**Array-likes** 是有{{c1:: 索引和 `length` 属性}}的对象，所以它们很像数组。

```javascript
//{{c1:: 
let arrayLike = { // 有索引和长度 => 类数组对象
  0: "Hello",
  1: "World",
  length: 2
};
}}
// 错误（没有 Symbol.iterator）
for (let item of arrayLike) {}
```

### `Array.from`  [	](javascript_info_20191219101334521)

有一个全局方法 [Array.from](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 可以以一个{{c1::  **可迭代对象**或者**类数组对象**}}作为参数并返回一个真正的 `Array` 数组。

```javascript
Array.from(obj[, mapFn, thisArg])
```

-  `mapFn` {{c1::  应是一个在元素被添加到数组前，施加于每个元素的方法 }}
- `thisArg` {{c1:: 允许设置方法的 `this` 对象 }}

```javascript
let arrayLike = {
  0: "Hello",
  1: "World",
  length: 2
};

let arr = Array.from(arrayLike); // (*)
alert(arr.pop()); // World（pop 方法生效）
```

## `Map、Set、WeakMap` 和 `WeakSet`  [	](javascript_info_20191219101334523)

### Map  [	](javascript_info_20191219101334524)

[Map](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Map) 是一个键值对的集合，很像 `Object`。但主要的区别是，`Map` 允许所有数据类型作为键。。

`NaN`{{c1::  也可以作为键}}

主要的方法包括：

- `new Map()` –{{c1::  创建 map。}}
- `map.set(key, value)` – {{c1:: 根据键（key）存储值（value），**可以链式调用**。}}
- `map.get(key)` – {{c1:: 根据键返回值，如果 map 中该键不存在，返回 `undefined`。}}
- `map.has(key)` – {{c1:: 如果键存在，返回 `true`，否则返回 `false`。}}
- `map.delete(key)` – {{c1:: 移除该键的值。}}
- `map.clear()` – {{c1:: 清空 map。}}
- `map.size` – {{c1:: 返回当前元素个数。}}
- `map.forEach` – {{c1:: 遍历map。}}

###  `Object.entries(obj)`  [	](javascript_info_20191219101334526)

{{c1:: 它可以返回一个对象的键值对数组}}

{{c1:: 

```javascript
let map = new Map(Object.entries({
  name: "John",
  age: 30
}));
```

}}

这里，`Object.entries` 返回了键值对数组：{{c1:: `[ ["name","John"], ["age", 30] ]`}}

### 有三种方法可以循环遍历 `map`：  [	](javascript_info_20191219101334528)

{{c1:: 

- `map.keys()` – 返回键的迭代器，
- `map.values()` – 返回值的迭代器，
- `map.entries()` – 返回 `[key, value]` 迭代器入口，`for..of` 循环会默认使用它。

注意：map的迭代顺序是值的插入顺序。

}}

### Set  [	](javascript_info_20191219101334529)

`Set` 是一个值的集合，这个集合中所有的值仅出现一次。

主要方法包括：

- `new Set(iterable)` – {{c1:: 创建 set，利用数组来创建是可选的（任何可迭代对象都可以）。}}
- `set.add(value)` – {{c1:: 添加值，返回 set 自身。}}
- `set.delete(value)` –{{c1::  删除值，如果该 `value` 在调用方法的时候存在则返回 `true` ，否则返回 `false`。}}
- `set.has(value)` – {{c1:: 如果 set 中存在该值则返回 `true` ，否则返回 `false`。}}
- `set.clear()` –{{c1::  清空 set。}}
- `set.size` – {{c1:: 元素个数。}}

我们可以使用 {{c1:: `for..of` 或者 `forEach` }}来循环查看 set

### `WeakMap`和 `WeakSet`  [	](javascript_info_20191219101334531)

- `WeakMap` ——  {{c1:: `Map` 的一个变体，仅允许对象作为键，并且当对象由于其他原因不可引用的时候将其删除。
  - 它不支持整体的操作：没有 `size` 属性，没有 `clear()` 方法，没有迭代器。}}

- `WeakSet` ——  {{c1::  `Set` 的一个变体，仅存储对象，并且当对象由于其他原因不可引用的时候将其删除。
- 同样不支持 `size/clear()` 和迭代器。}}

## 对象的键、值、项  [	](javascript_info_20191219101334535)

### `Object.keys、values、entries` 三个方法  [	](javascript_info_20191219101334538)

- [Object.keys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) ——  {{c1:: 返回一个包含该对象全部的键的**数组**。}}
- [Object.values(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/values) —— {{c1::  返回一个包含该对象全部的值的**数组**。}}
- [Object.entries(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) ——  {{c1:: 返回一个包含该对象全部 [key, value] 键值对的**数组**。}}

跟 map 的区别：

|          | Map                  | Object                                          |
| :------- | :------------------- | :---------------------------------------------- |
| 调用语法 | `{{c1::map.keys()`}} | {{c1::`Object.keys(obj)`，而不是 `obj.keys()`}} |
| 返回值   | {{c1::可迭代项}}     | {{c1::「真正的」数组}}                          |

`Object.keys/values/entries` 忽略  {{c1:: Symbol 类型}}的属性

## 解构赋值  [	](javascript_info_20191219101334540)

### 在方法参数列表中使用解构赋值  [	](javascript_info_20191219101334543)

```javascript
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};
//{{c1:: 
function showMenu({
  title = "Untitled",
  width: w = 100,  // width 赋值给 w
  height: h = 200, // height 赋值给 h
  items: [item1, item2] // items 第一个元素赋值给 item1, 第二个元素赋值给 item2
}) 
}}{
  alert( `${title} ${w} ${h}` ); // My Menu 100 200
  alert( item1 ); // Item1
  alert( item2 ); // Item2
}

showMenu(options);
```
注意：
- 对象属性需要相同的名字
- 数组元素名称任意但是要注意顺序

我们可以通过指定空对象 `{}` 为整个函数参数的默认值。

## 日期和时间  [	](javascript_info_20191219101334546)

### 日期对象  [	](javascript_info_20191219101334548)

构造函数：

`new Date()`

{{c1:: 不带参数 —— 创建一个表示当前日期和时间的 `Date` 对象：}}

`new Date(milliseconds)`

{{c1:: 创建一个 `Date` 对象，参数是从 1970-01-01 00:00:00 UTC+0 开始所经过的毫秒（一秒的千分之一）数。}}

`new Date(datestring)`

{{c1:: 如果只有一个参数，并且是字符串，那么该参数会通过 `Date.parse` 算法解析（下面会提到）。}}

`new Date(year, month, date, hours, minutes, seconds, ms)`

{{c1:: 创建一个 Date 对象，参数是当地时区的日期组合信息。只有前两个参数是必须的。}}

常用函数：

`Date.now()`:返回当前的时间戳。

`Date.parse()`:会转化一个特定格式的字符串，返回一个时间戳（自 1970-01-01 00:00:00 起的毫秒数），如果格式不正确，返回 `NaN`。

### 日期字符串格式（年-月-日 T 小时：分钟：秒：毫秒）  [	](javascript_info_20191219101334549)

{{c1:: 

字符串的格式是：`YYYY-MM-DDTHH:mm:ss.sssZ`，其中：

- `YYYY-MM-DD` —— 日期：年-月-日。
- 字符串 `"T"` 是一个分隔符。
- `HH:mm:ss.sss` —— 时间：小时，分钟，秒，毫秒。
- 可选字符 `'Z'` 代表时区。单个字符 `Z` 代表 UTC+0。

简短形式也是可以的，比如 `YYYY-MM-DD` 或者 `YYYY-MM` 又或者 `YYYY`。

}}

###  JavaScript 日期对象小结  [	](javascript_info_20191219101334552)

- 在 JavaScript 中，日期和时间使用 [Date](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date) 对象来表示{{c1:: 。不能只创建日期，或者只创建时间，`Date` 对象总是两个都创建。}}
- 月份从 {{c1:: 0 开始计数（对，一月是 0）。}}
- 一周的某一天 `getDay()` 同样从{{c1::  0 开始计算（0 代表星期天）。}}
- 当超出范围的信息被设置时，{{c1:: `Date` 会做自我校准。这一点对于日/月/小时 的加减很有效。}}
- 日期可以相减{{c1:: ，得到的是两者的差值，用毫秒表示。因为当转化为数字时，`Date` 对象变为时间戳。}}

## JSON 方法，toJSON  [	](javascript_info_20191219101334554)

### JSON 编码的对象与对象字面量有几个重要的区别：  [	](javascript_info_20191219101334555)

{{c1::

- 字符串使用双引号。JSON 中没有单引号或反引号。所以 `'John'` 转成 `"John"`。
- JSON对象属性名称也是双引号的。这是强制性的。所以 `age:30` 转成 `"age":30`。

}}

### js中将对象转换为字符串  [	](javascript_info_20191219101334556)

```javascript
let student = {
  name: 'John',
  age: 30,
  isAdmin: false,
  courses: ['html', 'css', 'js'],
  wife: null
};
//{{c1::
let json = JSON.stringify(student);
//}}
alert(typeof json); // we've got a string!
 }
alert(json);
/* JSON-encoded object:
{
  "name": "John",
  "age": 30,
  "isAdmin": false,
  "courses": ["html", "css", "js"],
  "wife": null
}
*/
```

###  `JSON.stringify` 会跳过的3种类型的值  [	](javascript_info_20191219101334558)

- {{c1::函数属性（方法）。}}

- {{c1::Symbolic 属性。}}

- {{c1::存储 `undefined` 的属性。}}解构赋值总结

{{c1::

```javascript
let user = {
  sayHi() { // ignored
    alert("Hello");
  },
  [Symbol("id")]: 123, // ignored
  something: undefined // ignored
};
alert( JSON.stringify(user) ); // {} (empty object)
```

}}

### `JSON.stringify` 的方法签名：  [	](javascript_info_20191219101334560)

{{c1::

```javascript
let json = JSON.stringify(value, replacer, space)
```

- value

  要编码的值。

- replacer

  要编码的属性数组**或**映射函数 `function(key, value)`。

- space

  文本添加缩进、空格和换行符

  这里 `spacer = 2` 告诉 JavaScript 在多行上显示嵌套对象，并在对象中缩进2个空格。
  

}}

### 对象中的` toJSON()`方法  [	](javascript_info_20191219101334561)

{{c1::

像 `toString` 进行字符串转换，对象可以提供 `toJSON` 方法来进行 JSON 转换。如果可用，`JSON.stringify` 会自动调用它。

}}

### ` JSON.parse(str, reviver);` :reviver参数例子：  [	](javascript_info_20191219101334563)

```javascript
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';
//将JSON中的属性为“date"的字符串转换成成日期对象
let meetup = JSON.parse(str,
                        //{{c1:: 
                        function(key, value) {
                          if (key == 'date') return new Date(value);
                          return value;
                        }}});

alert( meetup.date.getDate() ); // now works!
```

### Rest 参数（剩余参数）`...`  [	](javascript_info_20191219101334564)

**Rest 参数必须{{c1:: 放到参数列表的末尾}}**

下面的例子即把前两个参数定义为变量，同时把剩余的参数收集到 `titles` 数组中：

{{c1::

```javascript
function showName(firstName, lastName, ...titles) {
  alert( firstName + ' ' + lastName ); // Julius Caesar
  // titles 数组中包含了剩余的参数
  // 也就是有 titles = ["Consul", "Imperator"]
  alert( titles[0] ); // Consul
  alert( titles[1] ); // Imperator
  alert( titles.length ); // 2
}

showName("Julius", "Caesar", "Consul", "Imperator");
```

}}

### arguments变量  [	](javascript_info_20191219101334566)

- 含义：{{c1::所有的参数被按序放置的类数组且可遍历的对象}}
- 类数组且可遍历的对象不是数组，{{c1::不能调用对应的数组方法}}


###  `Array.from(obj)` 和 `[...obj]` 的2点差别：  [	](javascript_info_20191219101334567)

- {{c1::`Array.from` 同时适用于类数组对象和可遍历对象。}}
- {{c1::Spread 操作符只能操作可遍历对象。}}

### Rest参数与Spread操作符的区分  [	](javascript_info_20191219101334569)

- {{c1::若 `...` 出现在函数的参数列表，那它表示的就是 Rest 参数，它会把函数多余的实参收集到一个数组中。}}
- {{c1::若 `...` 出现在函数调用或类似的表达式中，那它就是 Spread 操作符，它会把一个数组展开为逗号分隔的元素列表。}}

### `var` 声明的变量与`let`声明的变量有两点主要区别：  [	](javascript_info_20191219101334570)

- {{c1::变量没有块作用域，它们在最小函数级可见。}}
- {{c1::变量声明在函数开头处理。}}

## 全局对象  [	](javascript_info_20191219101334572)

###  `<script type="module">  `的作用？  [	](javascript_info_20191219101334573)

{{c1::

可以将作用域与顶级作用域（window对象）分开。

`<script>` 与页面中的`JS`中的作用域默认为顶级作用域。

}}

## 函数对象  [	](javascript_info_20191219101334575)

### 函数对象的属性  [	](javascript_info_20191219101334576)

- name:{{c1::该函数的名字}}  
- length:{{c1::该函数的参数个数}}

- 自定义属性{{c1::

```javascript
function sayHi() {
  alert("Hi");

  // 我们记录一下运行次数
  sayHi.counter++;
}
sayHi.counter = 0; // 初始值

sayHi(); // Hi
sayHi(); // Hi

alert( `调用了 ${sayHi.counter} 次` ); // 调用了 2 次
```
}}

## 命名函数表达式（NFE）  [	](javascript_info_20191219101334577)

{{c1::

 如果函数是通过函数表达式被声明的（不是在主代码流里），它附带了名字，那么它被称为命名的函数表达式。可以用来在函数内部引用自己，或者递归调用等诸如此类场景。 

```javascript
let sayHi = function func(who) {
  alert(`Hello, ${who}`);
};
```

}}

### Lexical Environment [	](javascript_info_20191230080406348)

在 JavaScript 中，每个运行的函数、代码块或整个程序，都有一个称为{{c1:: **词法环境（Lexical Environment）** }}的关联对象。

词法环境对象由两部分组成：

1. **环境记录（Environment Record）**—— {{c1::  一个把所有局部变量作为其属性（包括一些额外信息，比如 `this` 值）的对象。}}
2. **外部词法环境（outer lexical environment）**的引用 —— {{c1::通常是嵌套当前代码（当前花括号之外）之外代码的词法环境。}}

总结一下：

- 变量是特定内部对象的属性，与当前执行的（代码）块/函数/脚本有关。
- 操作变量实际上操作的是该对象的属性。

**当代码试图访问一个变量时 —— 它首先会在内部词法环境中进行搜索，然后是外部环境，然后是更外部的环境，直到（词法环境）链的末尾。**

![image-20191224125817435](javascript_info.assets/image-20191224125817435.png)

### task: 为 `counter` 添加 `set` 和 `decrease` 方法  [	](javascript_info_20191219101334579)

```js
function makeCounter() {
  function counter() {
    return counter.count++;
  };
  counter.count = 0;
  return counter;
}

let counter = makeCounter();

counter.count = 10;
alert( counter() ); // 10
```

修改 `makeCounter()` 代码，使得 counter 可以减一和赋值：

- `counter()` 应该返回下一个数字（同以前逻辑）。
- `counter.set(value)` 应该设置 `count` 为 `value`。
- `counter.decrease(value)` 应该把 `count` 减 1。

P.S. 你也可以使用闭包或者函数属性来保持当前的计数，或者两者的变体

---

{{c1::

 该解在局部变量中使用 `count`，但是在 `counter` 中直接添加了方法。它们共享同一个外部词法环境，并且可以访问当前 `count`。 

```js
    function makeCounter() {
        function counter() {
            return counter.count++;
        };
        counter.count = 0;
        counter.set=function (value) {
            counter.count=value;
            return this.count;
        }
        counter.decrease=function () {
            counter.count--;
            return this.count;
        }
        return counter;
    }

    let counter = makeCounter();

    counter.count = 10;
    alert( counter() ); // 10
    alert( counter.decrease() ); // 10
    alert( counter.decrease() ); // 10
    alert( counter.decrease() ); // 10
    alert( counter.decrease() ); // 10
    alert( counter() ); // 10
```

}}

### task:任意多个括号求和   [	](javascript_info_20191219101334581)

写一个函数 `sum`，它有这样的功能：

```javascript
sum(1)(2) == 3; // 1 + 2
sum(1)(2)(3) == 6; // 1 + 2 + 3
sum(5)(-1)(2) == 6
sum(6)(-1)(-2)(-3) == 0
sum(0)(1)(2)(3)(4)(5) == 15
```

{{c1::

1. **无论**整体如何工作，`sum` 的结果必须是函数。
2. 这个函数必须在内存里保留调用之间的当前值。
3. 根据任务，当函数被用在 `==` 左右时，它必须返回数字。函数是对象，所以转换如 [对象原始值转换](https://zh.javascript.info/object-toprimitive) 章节所述，我们可以提供自己的方法来返回数字。

代码如下：

```javascript
function sum(a) {

  let currentSum = a;

  function f(b) {
    currentSum += b;
    return f;
  }

  f.toString = function() {
    return currentSum;
  };

  return f;
}

alert( sum(1)(2) ); // 3
alert( sum(5)(-1)(2) ); // 6
alert( sum(6)(-1)(-2)(-3) ); // 0
alert( sum(0)(1)(2)(3)(4)(5) ); // 15
```

请注意 `sum` 函数只工作一次，它返回了函数 `f`。

然后，接下来的每一次调用，`f` 都会把自己的参数加到求和 `currentSum` 上，然后返回自己。

}}

## new Function 语法  [	](javascript_info_20191219101334583)

###  使用构造函数 `new Function` 创建函数   [	](javascript_info_20191219101334584)

构造方法签名如下：

{{c1::

```javascript
let func = new Function(arg1, arg2, ..., body);
```

由于历史原因，参数也可以按逗号分隔符的形式给出。

以下三种形式表现一致：

```javascript
new Function('a', 'b', 'return a + b'); // 基础语法
new Function('a,b', 'return a + b'); // 逗号分隔
new Function('a , b', 'return a + b'); // 逗号和空格分隔
```

}}

###  new Function 语法的闭包  [	](javascript_info_20191219101334586)

使用 `new Function` 创建出来的函数，它的{{c1:: `[[Environment]]` 指向全局词法环境}}，而不是函数所在的外部词法环境。因此，我们不能在新函数中直接使用外部变量。不过这样也挺好，这有助于{{c1::减少我们代码中可能出现的错误}}。同时使用参数显式地传值也有助于维护良好的代码结构且避免了因使用 minifier 带来的问题。

## 调度：`setTimeout`和`setInterval`  [	](javascript_info_20191219101334588)

### `setTimeout`  [	](javascript_info_20191219101334589)



方法签名：{{c1::

```javascript
let timerId = setTimeout(func|code, delay[, arg1, arg2...])
```

参数说明：

`func|code` ：想要执行的函数或代码字符串。 一般传入的都是函数，介于某些历史原因，代码字符串也支持，但是不建议使用这种方式。

`delay` ：执行前的延时，以毫秒为单位（1000 毫秒 = 1 秒）；

`arg1`，`arg2`… ：要传入被执行函数（或代码字符串）的参数列表（IE9 以下不支持）

}}

调用例子：

 {{c1:: 

```javascript
setTimeout(sayHi, 1000, "Hello", "John"); // Hello, John
```

**要函数，但不要执行函数**}}



### `clearTimeout `  [	](javascript_info_20191219101334591)

 {{c1:: 

`setTimeout` 在调用时会返回一个“定时器 id”—— 例子中为变量 `timerId` 持有，接下来用它取消调度。

取消调度的语法：

```javascript
let timerId = setTimeout(...);
clearTimeout(timerId);
```

}}

### `setInterval`  [	](javascript_info_20191219101334592)

 {{c1:: 

`setInterval` 方法和 `setTimeout` 的用法是相同的：

```javascript
let timerId = setInterval(func|code, delay[, arg1, arg2...])
```

所有参数的意义也是相同的，不过相对于 `setTimeout` 只执行一次，`setInterval` 是每间隔一定时间周期性执行。

想要阻止后续调用，我们需要调用 `clearInterval(timerId)`。

}}

### 递归版` setTimeout`  [	](javascript_info_20191219101334597)

```javascript
/** 这是一种：
let timerId = setInterval(() => alert('tick'), 2000);
*/
//递归版` setTimeout`
//{{c1:: 
let timerId = setTimeout(function tick() {
  alert('tick');
  timerId = setTimeout(tick, 2000); // (*)
}, 2000);
}}
```

 **递归版 `setTimeout` 能保证每次执行间的延时都是准确的，`setInterval` 却不能够。** 

### `setTimeout(…,0)`  [	](javascript_info_20191219101334599)

 {{c1:: 

还有一种特殊的用法：`setTimeout(func, 0)`。

这样调度可以让 `func` 尽快执行，但是只有在当前代码执行完后，调度器才会对其进行调用。

下面例子中，代码会先输出 “Hello”，然后紧接着输出 “World”：

```javascript
setTimeout(() => alert("World"), 0);
alert("Hello");
```

}}

###  用 `setTimeout` 分割 CPU 高占用任务的技巧。   [	](javascript_info_20191219101334600)

**给浏览器渲染的机会**

```js
<div id="progress"></div>

<script>
  let i = 0;

  function count() {

	    // 每次只完成一部分 (*)
//{{c1:: 
      do {
        i++;
        progress.innerHTML = i;
      } while (i % 1e3 != 0);

      if (i < 1e9) {
        setTimeout(count, 0);
      }
}}
  }

  count();
</script>
```



## 装饰和转发，call/apply  [	](javascript_info_20191219101334602)

## 透明缓存: 装饰器  [	](javascript_info_20191219101334604)

```javascript
function slow(x) {
  // 这里可能会有重负载的CPU密集型工作
  alert(`Called with ${x}`);
  return x;
}

function cachingDecorator(func) {
  let cache = new Map();

  return function(x) {
//{{c1:: 
    if (cache.has(x)) { // 如果结果在 map 里
      return cache.get(x); // 返回它
    }

    let result = func(x); // 否则就调用函数

    cache.set(x, result); // 然后把结果缓存起来
    return result;
}}
  };
}

slow = cachingDecorator(slow);

alert( slow(1) ); // slow(1) 被缓存起来了
alert( "Again: " + slow(1) ); // 一样的

alert( slow(2) ); // slow(2) 被缓存起来了
alert( "Again: " + slow(2) ); // 也是一样
```

### 使用 `“func.call” `作为上下文  [	](javascript_info_20191219101334605)

一个特殊的内置函数方法 [func.call(context, …args)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Function/call)，允许调用一个显式设置 `this` 的函数。

语法如下：

 {{c1:: 

```javascript
func.call(context, arg1, arg2, ...)
```

```javascript
function sayHi() {
  alert(this.name);
}

let user = { name: "John" };
let admin = { name: "Admin" };

// 使用 call 将不同的对象传递为 "this"
sayHi.call( user ); // this = John
sayHi.call( admin ); // this = Admin
```

}}

### 使用 `“func.call” `实现装饰器  [	](javascript_info_20191219101334607)

```javascript
let worker = {
  someMethod() {
    return 1;
  },

  slow(x) {
    alert("Called with " + x);
    return x * this.someMethod(); // (*)
  }
};

function cachingDecorator(func) {
  let cache = new Map();
  
  return function(x) {
//{{c1:: 
      if (cache.has(x)) {
        return cache.get(x);
      }
      let result = func.call(this, x); // "this" 现在被正确的传递了
      cache.set(x, result);
      return result;
}}
  };
}

worker.slow = cachingDecorator(worker.slow); // 现在让他缓存起来

alert( worker.slow(2) ); // 生效了
alert( worker.slow(2) ); // 生效了, 不会调用原始的函数了。被缓存起来了
```

### 使用 `“func.apply” `来传递多参数  [	](javascript_info_20191219101334608)

 `call` 和 `apply` 之间唯一的语法区别是 {{c1::`call` 接受一个参数列表，而 `apply` 则接受带有一个类似数组的对象。 }}

这两个调用结果几乎相同：

```javascript
let args = [1, 2, 3];
//{{c1::
func.call( context, ...args); //}} 使用 spread 运算符将数组作为参数列表传递
{{c1::
func.apply( context, args);   //}} 与使用 apply 相同
```

###  `func.call` 和 `func.apply` 细微的差别。  [	](javascript_info_20191219101334609)

{{c1:: 

- 扩展运算符 `...` 允许将 **可迭代的** `参数列表` 作为列表传递给 `call`。
- `apply` 只接受 **类似数组一样的** `参数列表`。

}}

### `apply` 最重要的用途之一是将调用传递给另一个函数，如下所示：  [	](javascript_info_20191219101334611)

{{c1:: 

```javascript
let wrapper = function() {
  return anotherFunction.apply(this, arguments);
};
```

这叫做 **呼叫转移**。`wrapper` 传递它获得的所有内容：上下文 `this` 和 `anotherFunction` 的参数并返回其结果。

}}

## 函数绑定  [	](javascript_info_20191219101334612)

### task:对同一个函数调用两次` bind`  [	](javascript_info_20191219101334614)

下面代码输出是什么？

```javascript
function f() {
  alert(this.name);
}
//第二次调用` bind `改变 `this` 吗
f = f.bind( {name: "John"} ).bind( {name: "Ann" } );

f();
```

---

{{c1:: 

答案：**John**.

```javascript
function f() {
  alert(this.name);
}

f = f.bind( {name: "John"} ).bind( {name: "Pete"} );

f(); // John
```

`f.bind(...)` 返回的外来的 [绑定函数](https://tc19.github.io/ecma262/#sec-bound-function-exotic-objects) 对象仅在创建的时候记忆上下文（如果提供了参数）。

一个函数不能作为重复边界。

}}

### task:`bind` 过后的函数属性  [	](javascript_info_20191219101334615)

下面代码输出是什么？

```javascript
function sayHi() {
  alert( this.name );
}
sayHi.test = 5; //添加一个属性

let bound = sayHi.bind({  //调用bind
  name: "John"
});

alert( bound.test ); // 输出会是什么?
```

---

{{c1:: 

答案：`undefined`.

`bind` 的结果是另一个对象，它并没有 `test` 属性。

}}

### task:为什么 `this `会丢失  [	](javascript_info_20191219101334617)

下面代码中对 `askPassword()` 的调用将会检查密码然后基于结果调用 `user.loginOk/loginFail`。

但是它导致了一个错误。为什么？

```javascript
function askPassword(ok, fail) {
  let password = prompt("Password?", '');
  if (password == "rockstar") ok();
  else fail();
}

let user = {
  name: 'John',

  loginOk() {
    alert(`${this.name} logged in`);
  },

  loginFail() {
    alert(`${this.name} failed to log in`);
  },

};

askPassword(user.loginOk, user.loginFail);  //修改这一行代码,让一切开始正常运行
```

---

{{c1:: 

 `bind` 上下文：

```javascript
askPassword(user.loginOk.bind(user), user.loginFail.bind(user));
```

}}

另一个可以用来替换的解决办法是：

{{c1:: 

```javascript
//...
askPassword(() => user.loginOk(), () => user.loginFail());
```

}}

通常情况下它也能正常运行，但是可能会·在更复杂的场景下失效，例如在{{c1::  asking 到运行 `() => user.loginOk()` 之间，`user` 可能会被重写。}}

### 利用 `bind`  实现偏函数  [	](javascript_info_20191219101334621)

`bind` 的方法签名：

{{c1:: 

```javascript
let bound = func.bind(context, arg1, arg2, ...);
```

}}

举个例子，我们有一个做乘法运算的函数 `mul(a,b)`：

```javascript
function mul(a, b) {
  return a * b;
}
```

基于它，我们利用 `bind` 创造一个新函数 `double`：

```javascript
let double = mul.bind(null, 2);

alert( double(3) ); // = mul(2, 3) = 6
alert( double(4) ); // = mul(2, 4) = 8
alert( double(5) ); // = mul(2, 5) = 10
```

##  [属性的标志和描述符](https://zh.javascript.info/property-descriptors)       [	](javascript_info_20191219101334630)

### 对象属性除 **`value`** 外还有三个特殊属性（所谓的“标志”）： [	](javascript_info_20191230080406353)

{{c1::

- **`writable`** — 如果为 `true`，则可以修改，否则它是只读的。
- **`enumerable`** — 如果是 `true`，则可在循环中列出，否则不列出。
- **`configurable`** — 如果是 `true`，则此属性可以被删除，相应的特性也可以被修改，否则不可以。

}}

### 获得属性的标志  [	](javascript_info_20191219101334632)

```javascript
let user = {
  name: "John"
};

let descriptor ={{c1::  Object.getOwnPropertyDescriptor(user, 'name');}}

alert( JSON.stringify(descriptor, null, 2 ) );

/* property descriptor:
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```

{{c1::   [Object.getOwnPropertyDescriptor](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor) 方法 }}允许查询有关属性的**完整**信息。

```javascript
let descriptor =//{{c1::
                 Object.getOwnPropertyDescriptor(obj, propertyName);
//}}
```

### 修改属性的标志  [	](javascript_info_20191219101334634)

为了修改标志，我们可以使用 [Object.defineProperty](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)。

语法是：

```javascript
Object.defineProperty(obj, propertyName, descriptor)
```

```javascript
let user = {};
//{{c1:: 
Object.defineProperty(user, "name", {
  value: "John"
});
//}}

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": "John",
  "writable": false,
  "enumerable": false,
  "configurable": false
}
 */
```

 在非严格模式下，写入只读属性等时不会发生错误。但操作仍然不会成功。非严格模式下违反标志的行为只是默默地被忽略。

### `Object.defineProperties`  [	](javascript_info_20191219101334636)

有一个方法 {{c1:: [Object.defineProperties(obj, descriptors)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties)，}}允许一次定义多个属性。

例如：

```javascript
//{{c1:: 
  Object.defineProperties(user, {
    name: { value: "John", writable: false },
    surname: { value: "Smith", writable: false },
    // ...
  });
//}}
```

### `Object.getOwnPropertyDescriptors`  [	](javascript_info_20191219101334638)

要一次获取所有属性描述符，我们可以使用 [Object.getOwnPropertyDescriptors(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors) 方法。

与 `Object.defineProperties` 一起，它可以用作克隆对象的“标志感知”方式：{{c1:: 

```javascript
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```

}}

通常，当我们克隆一个对象时，我们使用赋值的方式来复制属性，如下所示：

```javascript
for (let key in user) {
  clone[key] = user[key]
}
```

…但是，这并不能复制标志。所以如果我们想要一个“更好”的克隆，那么 `Object.defineProperties` 是首选。

另一个区别是 `for..in` 忽略了 symbolic 属性，但是{{c1::  `Object.getOwnPropertyDescriptors` }}返回包含 symbolic 属性在内的**所有**属性描述符。

## 属性的 getter 和 setter  [	](javascript_info_20191219101334640)

### getter 和 setter简单使用  [	](javascript_info_20191219101334641)

```javascript
let user = {
  name: "John",
  surname: "Smith",
//{{c1:: 
    get fullName() {
      return `${this.name} ${this.surname}`;
    },
    set fullName(value) {
      [this.name, this.surname] = value.split(" ");
    }
//}}
};

// set fullName is executed with the given value.
user.fullName = "Alice Cooper";

alert(user.name); // Alice
alert(user.surname); // Cooper
```

### 访问器描述符  [	](javascript_info_20191219101334643)

对于访问器属性，没有 `value` 和 `writable`，但是有 `get` 和 `set` 函数。

所以访问器描述符可能有：

{{c1:: 

- **`get`** —— 一个没有参数的函数，在读取属性时工作，
- **`set`** —— 带有一个参数的函数，当属性被设置时调用，
- **`enumerable`** —— 与数据属性相同，
- **`configurable`** —— 与数据属性相同。}}

 如果我们试图在同一个描述符中提供 `get` 和 `value`，则会出现错误 

###  要使用 `defineProperty` 创建 `fullName` 的访问器   [	](javascript_info_20191219101334645)

```javascript
let user = {
  name: "John",
  surname: "Smith"
};
//{{c1:: 
  Object.defineProperty(user, 'fullName', {
    get() {
      return `${this.name} ${this.surname}`;
    },

    set(value) {
      [this.name, this.surname] = value.split(" ");
    }
  });
//}}
alert(user.fullName); // John Smith

for(let key in user) alert(key); // name, surname
```

#  原型，继承  [	](javascript_info_20191219101334647)

## 原型继承  [	](javascript_info_20191219101334648)

###  JS中的原型继承总结  [	](javascript_info_20191219101334654)

- JavaScript 中，所有的对象都有一个隐藏的 `[[Prototype]]` 属性，它可以是另一个对象或者 `null`。
- 我们可以使用 `obj.__proto__` 进行访问`[[Prototype]]` ，`[[Prototype]]` 引用的对象称为“原型”。

- 请注意 `__proto__` 与 `[[Prototype]]` **不一样**。这是一个 getter/setter。
- 如果我们想要读取 `obj` 属性或者调用一个方法，而且它不存在，那么 {{c1:: JavaScript 就会尝试在原型中查找它。写/删除直接在对象上进行操作，它们不使用原型（除非属性实际上是一个 setter）。}}
- 如果我们调用 `obj.method()`，而且 `method` 是从原型中获取的， {{c1::`this` 仍然会引用 `obj`。因此方法总是与当前对象一起工作，即使它们是继承的。}}


### task: 与原型一起工作  [	](javascript_info_20191219101334660)

如下创建一对对象的代码，然后对它们进行修改。

过程中显示了哪些值？

```javascript
let animal = {
  jumps: null
};
let rabbit = {
  __proto__: animal,
  jumps: true
};

alert( rabbit.jumps ); // ? (1)

delete rabbit.jumps;

alert( rabbit.jumps ); // ? (2)

delete animal.jumps;

alert( rabbit.jumps ); // ? (3)
```

应该有 3 个答案。{{c1:: 

---

1. `true`，来自于 `rabbit`。

2. `null`，来自于 `animal`。

3. `undefined`,不再有这样的属性存在。}}

## 函数原型  [	](javascript_info_20191219101334662)

###  `F.prototype`的使用？  [	](javascript_info_20191219101334665)

```javascript
let animal = {
  eats: true
};

function Rabbit(name) {
  this.name = name;
}
//{{c1:: 
Rabbit.prototype = animal;
let rabbit = new Rabbit("White Rabbit"); //  rabbit.__proto__ == animal
//}}
alert( rabbit.eats ); // true
```

设置 `Rabbit.prototype = animal` 的这段代码表达的意思是：“当 `new Rabbit` 创建时，把它的 `[[Prototype]]` 赋值为 `animal`” 。

###  函数默认的 `"prototype"`是什么?  [	](javascript_info_20191219101334666)

 函数默认的 `"prototype"` 是{{c1::一个只有属性 `constructor` 的对象，它指向函数本身。 }}

```javascript
function Rabbit() {}
//{{c1::
/* default prototype
Rabbit.prototype = { constructor: Rabbit };
*/
//}}
```

### 函数原型总结  [	](javascript_info_20191219101334668)

- `F.prototype` 属性与 `[[Prototype]]` 不同。`F.prototype` 唯一的作用是：{{c1::当 `new F()` 被调用时，它设置新对象的 `[[Prototype]]`。}}
- `F.prototype` 的值应该是{{c1::一个对象或 null：其他值将不起作用。}}
- Person类的原型实例图{{c1::
![image-20191226202831741](javascript_info.assets/image-20191226202831741.png)}}

### task:`new user.constructor('Pete')` 的工作原理是：  [	](javascript_info_20191219101334669)

```javascript
function User(name) {
  this.name = name;
}
User.prototype = {}; // (*)

let user = new User('John');
let user2 = new user.constructor('Pete');

alert( user2.name ); // undefined
```

为什么 `user2.name` 是 `undefined`？

---

{{c1::

`new user.constructor('Pete')` 的工作原理是：

1. 首先，它在 `user` 中寻找 `constructor`。什么也没有。
2. 然后它追溯原型链。`user` 的原型是 `User.prototype`，它也什么都没有。
3. `User.prototype` 的值是一个普通对象 `{}`，其原型是 `Object.prototype`。还有 `Object.prototype.constructor == Object`。所以就用它了。

最后，我们有 `let user2 = new Object('Pete')`。内置的 `Object` 构造函数忽略参数，它总是创建一个空对象 —— 这就是我们在 `user2` 中所拥有的东西。

}}

## 验证内置原型[	](javascript_info_20191230080406355)

```javascript
let arr = [1, 2, 3];
// it inherits from Array.prototype?
// true 与第一层原型比较
//{{c1::
alert( arr.__proto__ === Array.prototype ); 
//}}
// then from Object.prototype?
// true  与第二层原型比较
//{{c1::
alert( arr.__proto__.__proto__ === Object.prototype );
//}}
// and null on the top.
// null 与第二层原型比较
//{{c1::
alert( arr.__proto__.__proto__.__proto__ ); 
//}}
```

### 使用原型进行方法借用： [	](javascript_info_20191230080406356)

```javascript
function showArgs() {
  // 从数组借用 join 方法并在 arguments 的上下文中调用
  alert( [].join.call(arguments, " - ") );
}

showArgs("John", "Pete", "Alice"); // John - Pete - Alice
```

因为 `join` 方法在 `Array.prototype` 对象上，我们可以直接调用它并且重写上面的代码：

```javascript
function showArgs() {
  //{{c1::
  alert( Array.prototype.join.call(arguments, " - ") );
	//}}
}
```

## 原生的原型总结  [	](javascript_info_20191219101334671)

- 所有的内置对象都遵循一样的模式：
  - 方法都存储在{{c1::原型对象上（`Array.prototype`、`Object.prototype`、`Date.prototype` 等）。}}
  - 对象本身只存储{{c1::数据（数组元素、对象属性、日期）。}}
- 基本数据类型同样在{{c1::包装对象的原型}}上存储方法：{{c1::`Number.prototype`、`String.prototype` 和 `Boolean.prototype`。只有 `undefined` 和 `null` 没有包装对象。}}
- 内置对象的原型可以被修改或者被新的方法填充。但是这样做是不被推荐的。只有当添加一个还没有被 JavaScript 引擎支持的新方法的时候才可能允许这样做。

### 装饰器方法 “defer()”   [	](javascript_info_20191219101334673)
实现defer()方法，具有以下功能
```javascript
function f(a, b) {
  alert( a + b );
}
f.defer(1000)(1, 2); // 1 秒钟后显示 3
```

请注意参数应该被传给原函数。

---

```javascript
//{{c1::
Function.prototype.defer = function(ms) {
  let f = this;
  return function(...args) {
    setTimeout(() => f.apply(this, args), ms);
  }
};
//}}
// check it
function f(a, b) {
  alert( a + b );
}

f.defer(1000)(1, 2); // shows 3 after 1 sec
```

## 原型方法  [	](javascript_info_20191219101334675)

获取/设置原型的方式有很多，我们已知的有：

- [Object.create(proto\[, descriptors\])](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/create) —— {{c1::利用 `proto` 作为 `[[Prototype]]` 和可选的属性描述来创建一个空对象。}}
- [Object.getPrototypeOf(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf) ——{{c1:: 返回 `obj` 对象的 `[[Prototype]]`。}}
- [Object.setPrototypeOf(obj, proto)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf) ——{{c1:: 将 `obj` 对象的 `[[Prototype]]` 设置为 `proto`。}}

对原型的操作

```js
let animal = {
  eats: true
};
// 以 animal 为原型创建一个新对象
let rabbit = Object.create(animal);
alert(rabbit.eats); // true
alert(Object.getPrototypeOf(rabbit) === animal); // 获取 rabbit 的原型
Object.setPrototypeOf(rabbit, {}); // 将 rabbit 的原型更改为 {}
```

### 我们可以利用 `Object.create` 来实现比 `for..in` 循环赋值属性方式更强大的对象复制功能：  [	](javascript_info_20191219101334676)

```javascript
// obj 对象的浅复制

let clone = //{{c1::
	Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));
//}}
```

### 「极简」对象 [	](javascript_info_20191230080406358)

```javascript
//创建没有原型的极简对象
//{{c1::
let chineseDictionary = Object.create(null);
//}}
chineseDictionary.hello = "ni hao";
chineseDictionary.bye = "zai jian";

//和对象关系最密切的方法是 Object.something(...),极简对象也可以使用Object中的工具方法
//获取极简对象的键集合 {{c1::
alert(Object.keys(chineseDictionary)); // hello,bye}}
```

### 获取所有属性  [	](javascript_info_20191219101334677)

- [Object.keys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) / [Object.values(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/values) / [Object.entries(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) ——{{c1::返回一个数组，包含所有可枚举字符串属性名称/值/键值对。这些方法只会列出**可枚举**属性，而且它们**键名为字符串形式**。}}

如果我们想要 symbol 属性：

- [Object.getOwnPropertySymbols(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) —— {{c1::返回包含所有 symbol 属性名称的数组。}}

如果我们想要非可枚举属性：

- [Object.getOwnPropertyNames(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames) —— {{c1::返回包含所有字符串属性名的数组。}}

如果我们想要**所有**属性：

- [Reflect.ownKeys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) —— {{c1::返回包含所有属性名称的数组。}}

-  [obj.hasOwnProperty(key)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)：{{c1::如果 `obj` 有名为 `key` 的自身属性（而非继承），返回值为 `true`。}}

### task:原型方法调用方式的差异 [	](javascript_info_20191230080406360)

以下调用得到的结果是否相同？

```javascript
rabbit.sayHi();
Rabbit.prototype.sayHi();
Object.getPrototypeOf(rabbit).sayHi();
rabbit.__proto__.sayHi();
```

{{c1::

第一个调用中 `this == rabbit`，其他的 `this` 等同于 `Rabbit.prototype`，因为它是逗号之前的对象。

```javascript
function Rabbit(name) {
  this.name = name;
}
Rabbit.prototype.sayHi = function() {
  alert( this.name );
}

let rabbit = new Rabbit("Rabbit");

rabbit.sayHi();                        // Rabbit
Rabbit.prototype.sayHi();              // undefined
Object.getPrototypeOf(rabbit).sayHi(); // undefined
rabbit.__proto__.sayHi();              // undefined
```

}}

## 类 [	](javascript_info_20191230080406363)

### 什么是 class？ [	](javascript_info_20191230080406365)

```javascript
class User {
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name); }
}
// 类是函数
//{{c1::
alert(typeof User); // function}}
// ...或者，更确切地说是构造方法
//{{c1::
alert(User === User.prototype.constructor); // true}}
// User.prototype 中的方法，比如：
//{{c1::
alert(User.prototype.sayHi); // alert(this.name);}}
// 实际上在原型中有两个方法
//{{c1::
alert(Object.getOwnPropertyNames(User.prototype)); // constructor, sayHi}}
```

### 基本的类语法看起来是这样的： [	](javascript_info_20191230080406367)

```javascript
class MyClass {
  //{{c1::
  prop = value; // field

  constructor(...) { // 构造器
    // ...
  }

  method(...) {} // 方法

  get something(...) {} // getter 方法
  set something(...) {} // setter 方法

  [Symbol.iterator]() {} // 计算 name/symbol 名方法
  // ...}}
}
```

技术上来说，`MyClass` 是一个函数（我们提供作为 `constructor` 的那个），而 methods，getters 和 settors 都被写入 `MyClass.prototype`。

### 调用父类的方法 [	](javascript_info_20191230080406369)

- 执行{{c1:: `super.method(...)` }}调用父类方法。
- 执行{{c1:: `super(...)` }}调用父类构造函数（只能在子类的构造函数中运行）

### 如果一个类继承了另一个类并且没有 `constructor`，那么将生成以下“空” `constructor`： [	](javascript_info_20191230080406370)

```javascript
class Rabbit extends Animal {
  // 为没有构造函数的继承类生成以下的构造函数
  //{{c1::
  constructor(...args) {
    super(...args);
  }
  //}}
}
```

### 调用父类方法时`Error: Maximum call stack size exceeded`的原因,以及如何解决? [	](javascript_info_20191230080406373)

```javascript
let animal = {
  name: "Animal",
  eat() {
    alert(`${this.name} eats.`);
  }
};

let rabbit = {
  __proto__: animal,
  eat() {
    // ...围绕 rabbit-style 和 调用父类（animal）方法
    this.__proto__.eat.call(this); // (*)
  }
};

let longEar = {
  __proto__: rabbit, 
  eat() {
    // ...用长耳朵做一些事情，并调用父类（rabbit）的方法
    this.__proto__.eat.call(this); // (**)
  }
};

longEar.eat(); // Error: Maximum call stack size exceeded
```

- {{c1::为了提供解决方法，JavaScript 为函数额外添加了一个特殊的内部属性：`[[HomeObject]]`。

  当一个函数被定义为类或者对象方法时，它的 `[[HomeObject]]` 属性就成为那个对象。

  然后 `super` 使用它来解析父类原型和它自己的方法。}}

- {{c1::在 (*) 和 (**) 这两行中，`this.__proto__` 的值是完全相同的：都是 `rabbit`。在这个无限循环中，他们都调用了 `rabbit.eat`，而不是在原型链上向上寻找方法。}}

### 方法，不是函数属性，以下代码出错原因 [	](javascript_info_20191230080406377)

```javascript
let animal = {
  eat: function() { // 可以使用简短写法：eat() {...}
    // ...
  }
};

let rabbit = {
  __proto__: animal,
  eat: function() {
    super.eat();
  }
};

rabbit.eat();  // 错误调用 super（因为这里并没有 [[HomeObject]]）
```

{{c1::

- `[[HomeObject]]` 是为类和普通对象中的方法定义的。但是对于对象来说，方法必须确切指定为 `method()`，而不是 `"method: function()"`。
- 这个差别对我们来说可能不重要，但是对 JavaScript 来说却是非常重要的。}}

### JS中的类继承 [	](javascript_info_20191230080406379)

1. 扩展类：

   ```javascript
   class Child extends Parent
   ```

   - 这就意味着{{c1:: `Child.prototype.__proto__` 将是 `Parent.prototype`}}，所以方法被继承。

2. 重写构造函数：

   - 在使用 `this` 之前，我们必须在{{c1::  `Child` 构造函数中将父构造函数调用为 `super()`。}}

3. 重写方法：

   - 我们可以在 `Child` 方法中使用{{c1::  `super.method()` }}来调用 `Parent` 方法。

4. 内部工作：

   - 方法在内部{{c1::  `[[HomeObject]]` }}属性中记住它们的类/对象。这就是 `super` 如何解析父类方法的。
   - 因此，将一个带有 `super` 的方法从一个对象复制到另一个对象是不安全的。

补充：

- 箭头函数没有自己的 `this` 或 `super`，所以它们能融入到就近的上下文，像透明似的。

### `class Rabbit`与`class Rabbit extends Object`声明的区别 [	](javascript_info_20191230080406381)

|                                         | class Rabbit                                      | class Rabbit extends Object                    |
| --------------------------------------- | :------------------------------------------------ | :--------------------------------------------- |
| **构造器：**                            | {{c1:: – }}                                       | {{c1::needs to call `super()` in constructor}} |
| **原型：                             ** | {{c1::`Rabbit.__proto__ === Function.prototype`}} | {{c1::`Rabbit.__proto__ === Object`}}          |

### js中的静态类 [	](javascript_info_20191230080406383)

```javascript
class MyClass {
  static property = ...;

  static method() {
    ...
  };
}
```

技术上来说，静态声明等同于直接给类本身赋值：

```javascript
//如下所示
//{{c1::
MyClass.property = ...
MyClass.method = ...
//}}
```

静态属性和方法是可以被继承的。

### 私有的受保护属性 [	](javascript_info_20191230080406385)

为了隐藏内部接口，我们使用受保护的或私有的属性：

- 受保护的字段以 {{c1::`_`}} 开头。
- 私有字段以 {{c1::`#` }}开头。JavaScript 确保我们只能访问类中的内容。
- 私有字段不与公共字段发生冲突。{我们可以同时拥有{c1::私有属性 `#waterAmount` 和公共属性 `waterAmount`。}}

目前，在各浏览器中不支持私有字段，但可以用 polyfill 解决。

### 继承内置类 [	](javascript_info_20191230080406386)

如果我们希望类似 `map` 或者 `filter` 这样的内置方法返回常规的数组，我们应该在 `Symbol.species` 中返回 `Array`，就像这样：

```javascript
class PowerArray extends Array {
  isEmpty() {
    return this.length === 0;
  }

  // 内置方法会使用这个作为构造函数 (constructor)
  //{{c1::
  static get [Symbol.species]() {
    return Array;
  }
  //}}
}

let arr = new PowerArray(1, 2, 5, 10, 50);
alert(arr.isEmpty()); // false

// filter 使用 arr.constructor[Symbol.species] 作为构造函数 (constructor) 创建新数组
let filteredArr = arr.filter(item => item >= 10);

// filteredArr 不是 PowerArray, 而是 Array
alert(filteredArr.isEmpty()); // Error: filteredArr.isEmpty is not a function
```

### 内置类的静态方法不被继承 [	](javascript_info_20191230080406387)

- 比如，`Array` 和 `Data` 都是继承自 `Object`，所以它们的实例都有来自 `Object.prototype` 的方法
- 但是{{c1:: `Array.[[Prototype]]` 不指向 `Object`，所以它们没有例如 `Array.keys()`(或者 `Data.keys()`)的静态方法。}}

### 类型检测：静态方法 `Symbol.hasInstance` 的使用例子 [	](javascript_info_20191230080406389)
```javascript
// 假设具有 canEat 属性的对象为动物类
class Animal {
  //{{c1::
  static [Symbol.hasInstance](obj) {
    if (obj.canEat) return true;
  }
  //}}
}

let obj = { canEat: true };
alert(obj instanceof Animal); // 返回 true：调用 Animal[Symbol.hasInstance](obj)
```

### `obj instanceof Class` 语句的大致执行过程如下： [	](javascript_info_20191230080406391)

- {{c1::如果提供了静态方法 `Symbol.hasInstance`，那就直接用这个方法进行检测}}

- {{c1::没有 `Symbol.hasInstance` 方法，这时会检查 `Class.prototype` 是否与 `obj` 的原型链中的任何一个原型相等。}}

-  [objA.isPrototypeOf(objB)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/object/isPrototypeOf):{{c1::如果 `objA` 处在 `objB` 的原型链中，调用结果为 `true`。}}


### 技巧：使用 Object 的 toString 方法来揭示类型 [	](javascript_info_20191230080406393)

```javascript
let s = Object.prototype.toString;
//注意这里是调用的call，参数作为context用。{{c1::
alert( s.call(123) ); // [object Number]
alert( s.call(null) ); // [object Null]
alert( s.call(alert) ); // [object Function]}}
```

### 自定义 obj.toString 方法中揭示类型[	](javascript_info_20191230080406395)

- 使用{{c1::  Symbol.toStringTag }}这个特殊的对象属性进行自定义输出。

```javascript
//{{c1::
let user = {
  [Symbol.toStringTag]: "User"
};
//}}
alert( {}.toString.call(user) ); // [object User]
```

### 类型检测的方式 [	](javascript_info_20191230080406397)

| 类型检测方式  | 用于                                                         | 返回               |
| :------------ | :----------------------------------------------------------- | ------------------ |
| `typeof`      | {{c1::基本数据类型}}                                         | {{c1::string}}     |
| `{}.toString` | {{c1::基本数据类型、内置对象以及包含 `Symbol.toStringTag` 属性的对象}} | {{c1::string}}     |
| `instanceof`  | {{c1::任意对象}}                                             | {{c1::true/false}} |

`{}.toString` 基本就是一增强版 `typeof`。

`instanceof` 在涉及多层类结构的场合中比较实用，这种情况下需要将类的继承关系考虑在内。

### EventMixin [	](javascript_info_20191230080406400)

> 思考拓展:如果同时订阅多个相同`select`事件，不同事件处理器，处理器的执行顺序如何自定义?

- `.on(eventName, handler)` — 分配给事件处理器给某个事件。
- `.off(eventName, handler)` — 在事件处理函数列表中移除指定的函数。
- `.trigger(eventName, ...args)` — 触发事件：所有被指定到对应事件的事件处理函数都会被调用并且 `args` 会被作为参数传递给它们。

```javascript
let eventMixin = {
  /**
   * 订阅事件，用法：
   *  menu.on('select', function(item) { ... }
  */
  on(eventName, handler) {
    //{{c1::
    if (!this._eventHandlers) this._eventHandlers = {};
    if (!this._eventHandlers[eventName]) {
      this._eventHandlers[eventName] = [];
    }
    this._eventHandlers[eventName].push(handler);
    //}}
  },

  /**
   * 取消订阅，用法：
   *  menu.off('select', handler)
   */
  off(eventName, handler) {
    //{{c1::
    let handlers = this._eventHandlers && this._eventHandlers[eventName];
    if (!handlers) return;
    for (let i = 0; i < handlers.length; i++) {
      if (handlers[i] === handler) {
        handlers.splice(i--, 1);
      }
    }
    //}}
  },

  /**
   * 触发事件并传递参数
   *  this.trigger('select', data1, data2);
   */
  trigger(eventName, ...args) {
    //{{c1::
    if (!this._eventHandlers || !this._eventHandlers[eventName]) {
      return; // 对应事件名没有事件处理函数。
    }

    // 调用事件处理函数
    this._eventHandlers[eventName].forEach(handler => handler.apply(this, args));
    //}}
  }
};
```

使用例：

```javascript
// 新建一个 class
class Menu {
  choose(value) {
    this.trigger("select", value);
  }
}
// 添加 mixin
Object.assign(Menu.prototype, eventMixin);

let menu = new Menu();

// 被选中时调用事件处理函数：
menu.on("select", value => alert(`Value selected: ${value}`));

// 触发事件 => 展示被选中的值：123
menu.choose("123"); // 被选中的值
```

## `try..catch` 结构 [	](javascript_info_20191230080406402)

语法如下：{{c1::

```javascript
try {
    // 执行此处代码
  } catch(err) {
    // 如果发生异常，跳到这里
    // err 是一个异常对象
  } finally {
    // 不管 try/catch 怎样都会执行
  }
```

}} 

可能会没有 `catch` 代码块，或者没有 `finally` 代码块。所以 `try..catch` 或者 `try..finally` 都是可用的。

异常对象包含下列属性：

- `message` —— {{c1::我们能阅读的异常提示信息。}} 
- `name` ——{{c1:: 异常名称（异常对象的构造函数的名称）。}} 
- `stack`（没有标准） ——{{c1:: 异常发生时的调用栈。}} 

### 全局 catch  ` window.onerror`的使用 [	](javascript_info_20200114084259605)

```js
  //{{c1::
  window.onerror = function(message, url, line, col, error) {
    alert(`${message}\n At ${line}:${col} of ${url}`);
  };
  //}}

  function readData() {
    badFunc(); // 哦，出问题了！
  }

  readData();
```

### task:自定义异常：继承 SyntaxError  [	](javascript_info_20200114084259607)

创造一个继承自内置类 `SyntaxError` 的 `FormatError` 类。

它应该支持 `message`，`name` 和 `stack` 属性。

用例：

```javascript
let err = new FormatError("formatting error");

alert( err.message ); // formatting error
alert( err.name ); // FormatError
alert( err.stack ); // stack

alert( err instanceof FormatError ); // true
alert( err instanceof SyntaxError ); // true（因为它继承自 SyntaxError）
```

解决方案:{{c1::

```javascript
class FormatError extends SyntaxError {
  constructor(message) {
    super(message);
    this.name = "FormatError"; 
    // 更通用的方式，使之后继承的类不必再次设置this.name
    // this.name = this.constructor.name; 
  }
}

let err = new FormatError("formatting error");

alert( err.message ); // formatting error
alert( err.name ); // FormatError
alert( err.stack ); // stack

alert( err instanceof SyntaxError ); // true
```

}}

## 异步编程 [	](javascript_info_20200114084259608)

### Promise 对象的构造方法签名是： [	](javascript_info_20200114084259610)

```javascript
//{{c1::
let promise = new Promise(function(resolve, reject) {
  // executor (生产者代码，"singer")
});
//}}
```

### `promise` 对象有内部属性： [	](javascript_info_20200114084259612)

- `state` —— {{c1:: 最初是 “pending”，然后被改为 “fulfilled” 或 “rejected” }}
- `result` —— {{c1:: 一个任意值，最初是 `undefined`。}}

### 当`promise`中`executor` 完成任务时，应调用以下两个方法之一： [	](javascript_info_20200114084259614)

`resolve(value)` ——{{c1:: 说明任务已经完成：}}

  - {{c1:: 将 `state` 设置为 `"fulfilled"` }}
  - {{c1:: 将`result` 设置为 `value` }}

 `reject(error)` —— {{c1:: 表明有错误发生：}}

  - {{c1:: 将 `state` 设置为 `"rejected"`}}
  - {{c1:: 将 `result` 设置为 `error`}}

### 使用promise示例：loadScript [	](javascript_info_20200114084259616)

```javascript
function loadScript(src) {
  //{{c1::
  return new Promise(function(resolve, reject) {
    let script = document.createElement('script');
    script.src = src;

    script.onload = () => resolve(script);
    script.onerror = () => reject(new Error("Script load error: " + src));

    document.head.append(script);
  });
  //}}
}
```

用法：

```javascript
let promise = loadScript("https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js");

promise.then(
  script => alert(`${script.src} is loaded!`),
  error => alert(`Error: ${error.message}`)
);

promise.then(script => alert('One more handler to do something else!'));
```

### task:下列代码会输出什么？ [	](javascript_info_20200114084259618)

```javascript
let promise = new Promise(function(resolve, reject) {
  resolve(1);

  setTimeout(() => resolve(2), 1000);
});

promise.then(alert);
```

---

{{c1::

输出为：`1`。

对 `resolve` 的第二次调用会被忽略，因为只有对 `reject/resolve` 的第一次调用会被处理。更深层的调用都会被忽略。

}}

### **The `state` and `result` are internal** [	](javascript_info_20200114084259620)

{{c1::

Promise 的 `state` 和 `result` 属性是内部的。我们不能从代码中直接访问它们，但是我们可以使用 `.then/catch` 来访问

调用 `.catch(f)` 是 `.then(null, f)` 的模拟，这只是一个简写。

}}

### promises链中，`.then`方法3种返回值 [	](javascript_info_20200114084259621)

- {{c1:: 作为`result`传给下一个then的属性}}
- {{c1:: 原生的`Promise`的对象 }}
- {{c1:: thenable 对象（一个具有 `.then` 方法的任意对象）}}

### promises链中,thenable 对象的处理 [	](javascript_info_20200114084259623)

- 执行流程
  - {{c1:: JavaScript 会检查 promises链中`.then` 方法返回的对象。}}
  - {{c1:: 如果它有一个名为 `then` 的可调用方法，那么它会调用该方法并提供原生函数 `resolve`，`reject `作为参数（类似于 executor）并在它被调用前一直等待。}}

### fetch的promises链调用示例 [	](javascript_info_20200114084259624)

- 作为一个规律，一个异步动作应该永远返回一个 promise。

```javascript
function loadJson(url) {
  // {{c1::
  return fetch(url)
    .then(response => response.json());
  // }}
}

function loadGithubUser(name) {
  // {{c1::
  return fetch(`https://api.github.com/users/${name}`)
    .then(response => response.json());
  // }}
}

function showAvatar(githubUser) {
  // {{c1::
  return new Promise(function(resolve, reject) {
    let img = document.createElement('img');
    img.src = githubUser.avatar_url;
    img.className = "promise-avatar-example";
    document.body.append(img);

    setTimeout(() => {
      img.remove();
      resolve(githubUser);
    }, 3000);
  });
  // }}
}

// 使用它们
loadJson('/article/promise-chaining/user.json')
  .then(user => loadGithubUser(user.name))
  .then(showAvatar)
  .then(githubUser => alert(`Finished showing ${githubUser.name}`));
  // ...
```

### 全局`unhandledrejection` 事件来捕获Promise中的异常  [	](javascript_info_20200114084259626)

```javascript
//{{c1::
window.addEventListener('unhandledrejection', function(event) {
  // the event object has two special properties:
  alert(event.promise); // [object Promise] - 产生错误的 promise
  alert(event.reason); // Error: Whoops! - 未处理的错误对象
});
//}}
new Promise(function() {
  throw new Error("Whoops!");
}); // 没有 catch 处理错误
```

### Promise中setTimeout 里的异常 [	](javascript_info_20200114084259627)

你怎么看？`.catch` 会触发么？解释你的答案。

```javascript
new Promise(function(resolve, reject) {
  setTimeout(() => {
    throw new Error("Whoops!");
  }, 1000);
}).catch(alert);
```

---

{{c1::

答案是：**不，它不会触发**：

```javascript
new Promise(function(resolve, reject) {
  setTimeout(() => {
    throw new Error("Whoops!");
  }, 1000);
}).catch(alert);
```

正如本章所述，函数代码周围有个“隐式 `try..catch`”。所以所有同步错误都被处理。

但是这里的错误并不是在执行阶段生成的，而是在执行阶段之后才生成错误。所以 promise 无法处理它。

}}

## `Promise` 类的 5 种静态方法 [	](javascript_info_20200308041234740)

### `Promise.resolve`静态方法 [	](javascript_info_20200114084259629)

用途：{{c1:: 当我们已经有一个 value 的时候，就会使用该方法，但希望将它“封装”进 promise。}}

例如，下面的 `loadCached` 函数会获取 `url` 并记住结果，以便以后对同一 URL 进行调用时可以立即返

```javascript
function loadCached(url) {
  let cache = loadCached.cache || (loadCached.cache = new Map());
	//{{c1::
  if (cache.has(url)) {
    return Promise.resolve(cache.get(url)); // (*)
  }
  //}}
  //{{c1::
  return fetch(url)
    .then(response => response.text())
    .then(text => {
      cache.set(url,text);
      return text;
    });
  //}}
}
```

### `Promise.reject`静态方法 [	](javascript_info_20200114084259630)

语法：

```javascript
let promise = Promise.reject(error);
```

等价于：

```javascript
// {{c1::
let promise = new Promise((resolve, reject) => reject(error));
// }}
```

### `Promise.all`静态方法 [	](javascript_info_20200114084259632)

- 用途:假设我想要并行执行多个 promise，并等待所有 promise 准备就绪。
- 它们的相对顺序是相同的。
- **如果任意一个 promise 为 reject，`Promise.all` 返回的 promise 就会立即 reject 这个错误。**
- **如果出现错误，其他 promise 就会被忽略**

```javascript
Promise.all(
    //{{c1::
      [
      new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
      new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
      new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3	
	  ]
    //}}
).then(alert); // 1,2,3 当 promise 就绪：每一个 promise 即成为数组中的一员
```

### `Promise.all(iterable)` 允许“迭代”中的非 promise（non-promise）的 “常规“ [	](javascript_info_20200308041234742)

```javascript
Promise.all([
  //{{c1::
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(1), 1000)
  }),
  2, // 视为 Promise.resolve(2)
  3  // 视为 Promise.resolve(3)
  //}}
]).then(alert); // 1, 2, 3
```

### `Promise.allSettled`静态方法 [	](javascript_info_20200308041234744)

```javascript
let urls = [
  'https://api.github.com/users/iliakan',
  'https://api.github.com/users/remy',
  'https://no-such-url'
];
//使用promise异步访问以上3个链接，并打印出链接与状态码或者错误。
Promise.allSettled(urls.map(url => fetch(url)))
       .then(results => { // (*)
            results.forEach((result, num) => {
              if (result.status == "fulfilled") {
                alert(`${urls[num]}: ${result.value.status}`);
              }
              if (result.status == "rejected") {
                alert(`${urls[num]}: ${result.reason}`);
              }
            });
       });
```

上面的 `(*)` 行，`results` 将会是：

```javascript
// {{c1::
[
  {status: 'fulfilled', value: ...response...},
  {status: 'fulfilled', value: ...response...},
  {status: 'rejected', reason: ...error object...}
]
//}}
```

返回所有promise的处理结果，返回的数组元素有2种类型：

- {{c1::`{status:"fulfilled", value:result}` 对于成功的响应。}}
- {{c1::`{status:"rejected", reason:error}` 对于错误的响应。}}

### 如果浏览器不支持 `Promise.allSettled`的，使用`promise.all`的替代方式 [	](javascript_info_20200308041234746)

```js
if(!Promise.allSettled) {
  Promise.allSettled = function(promises) {
    //{{c1::
    return Promise.all(promises.map(p => Promise.resolve(p).then(v => ({
      state: 'fulfilled',
      value: v,
    }), r => ({
      state: 'rejected',
      reason: r,
    }))));
    //}}
  };
}
```

### `Promise.race` [	](javascript_info_20200308041234747)

- 在第一个 promise 被解决（“赢得比赛[wins the race]”）后，所有后面的结果/错误都会被忽略。

```js
Promise.race([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("Whoops!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).then(alert); // 1
```

### 微任务队列（Microtasks queue） [	](javascript_info_20200308041234749)

- {{c1:: Promise 处理始终是异步的，因为所有 promise 操作都被放入内部的“promise jobs”队列执行，也被称为“微任务队列”（v8 术语）。}}

- {{c1:: 当一个 promise 准备就绪时，它的 `.then/catch/finally` 处理程序就被放入队列中。但是不会立即被执行。当 JavaScript 引擎执行完当前的代码，它会从队列中获取任务并执行它。}}


以下程序的结果?

```javascript
let promise = Promise.resolve();

promise.then(() => alert("promise done"));

alert("code finished"); 
```

{{c1:: 先会看到 `code finished` 的消息，然后才是 promise done,{{c1:: 微任务比 `setTimeout` 具有更高的优先级}}。 }}

## 未处理的 rejection [	](javascript_info_20200308041234750)

```javascript
let promise = Promise.reject(new Error("Promise Failed!"));
setTimeout(() => promise.catch(err => alert('caught')));

// Error: Promise Failed!
window.addEventListener('unhandledrejection', event => alert(event.reason));
```

上面代码的运行含义

{{c1::

现在，如果你运行以上的代码，我们先会看到 `Promise Failed!` 的消息，然后才是 `caught`。

如果我们并不了解微任务队列，我们可能想知道：“为什么 `unhandledrejection` 的处理程序会运行？我们已经去捕捉（catch）这个错误了！”

但是现在我们知道 `unhandledrejection` 在 microtask 队列完成时才会被生成：引擎会检查 promise，如果其中的任何一个出现“rejected”状态，`unhandledrejection` 事件就会被触发。

在这个例子中，被添加到 `setTimeout` 的 `.catch` 也会执行，只是会在 `unhandledrejection` 事件出现之后才执行，所以并没有改变什么（没有发挥作用）。

}}

### `async`关键值的作用： [	](javascript_info_20200308041234752)

```javascript
async function f() {
  return 1;
}
f().then(alert); // 1
```

相当于以下代码：

```javascript
//{{c1::
new Promise(resolve ->{
	resolve(1);
}).then(alert);
//}}
```

### Await关键值的使用: [	](javascript_info_20200308041234753)

最简单的例子：

```javascript
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  });

  let result = await promise; // 等待直到 promise 决议 (*)

  alert(result); // "done!"
}

f();
```

**不能在普通函数中使用** `await`：{{c1:: 在非 async 函数中使用 `await` 的话，就会报语法错误 }}

### `await` **不能在顶层代码运行** [	](javascript_info_20200308041234755)

```javascript
// 用在顶层代码中会报语法错误
let response = await fetch('/article/promise-chaining/user.json');
let user = await response.json();
```

解决办法：可以将其包裹在一个匿名 async 函数中

```javascript
//{{c1::
(async () => {
  let response = await fetch('/article/promise-chaining/user.json');
  let user = await response.json();
  ...
})();
// }}
```

### **Async methods** [	](javascript_info_20200308041234757)

如果想定义一个 async 的类方法，在方法前面添加 `async` 就可以了：

```javascript
//{{c1::
class Waiter {
  async wait() {
    return await Promise.resolve(1);
  }
}
//}}
new Waiter()
  .wait()
  .then(alert); // 1
```

## Async 中的错误处理 [	](javascript_info_20200308041234759)

```javascript
async function f() {
  throw new Error("Whoops!");
}
```

相当于如下代码：

```javascript
//{{c1::
async function f() {
  await Promise.reject(new Error("Whoops!"));
}
//}}
```

如果我们不使用 `try..catch`，由`f()` 产生的 promise 就会被拒绝。我们可以在函数调用后添加 `.catch` 来处理错误

当我们使用 `async/await` 时，几乎就不会用到 `.then` 了，因为为我们 `await` 处理了异步等待。并且我们可以用 `try..catch` 来替代 `.catch`。这通常更加方便（当然不是绝对的）。

但是当我们在顶层代码，外面并没有任何 `async` 函数，我们在语法上就不能使用 `await` 了，所以这时候就可以用 `.then/catch` 来处理结果和异常。

### `async/await` **可以和** `Promise.all` **一起使用** [	](javascript_info_20200308041234760)

当我们需要同时等待多个 promise 时，我们可以用 `Promise.all` 来包裹他们，然后使用 `await`：

```javascript
// 等待多个 promise 结果
//{{c1::
let results = await Promise.all([
  fetch(url1),
  fetch(url2),
  ...
]);
//}}
```

如果发生错误，也会正常传递：先从失败的 promise 传到 `Promise.all`，然后变成我们能用 `try..catch` 处理的异常。

### `async` 使用总结 [	](javascript_info_20200308041234761)

函数前面的关键字 `async` 有两个作用：

1. {{c1::让这个函数返回一个 promise。}}
2. {{c1::允许在函数内部使用 `await`。}}

这个 `await` 关键字又让 JavaScript 引擎等待直到 promise 完成，然后：

1. {{c1::如果有错误，就会抛出异常，就像那里有一个 `throw error` 语句一样。}}
2. {{c1::否则，就返回结果，并赋值。}}