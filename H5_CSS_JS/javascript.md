# JavaScript

[TOC]

## 1. 基础知识

### 1.1 Hello,world!

> 一般来说，只有最简单的脚本才嵌入到 HTML 中。更复杂的脚本存放在单独的文件中。
>
> 使用独立文件的好处是浏览器会下载它，然后将它保存到浏览器的缓存中。
>
> 之后，其他页面想要相同的脚本就会从缓存中获取，而不是下载它。所以文件实际上只会下载一次。
>
> 这可以节省流量，并使得页面（加载）更快。

- **如果设置了** `src` **特性，**`script` **标签内容将会被忽略。**

  - 一个单独的 `<script>` 标签不能同时有 `src` 特性和内部包裹的代码。

  - 这将不会工作：

    ```html
    <script src="file.js">
      alert(1); // 此内容会被忽略，因为设定了 src
    </script>
    ```

  - 我们必须进行选择，要么使用外部的 `<script src="…">`，要么使用正常包裹代码的 `<script>`。

    为了让上面的例子工作，我们可以将它分成两个 `<script>` 标签。

    ```html
    <script src="file.js"></script>
    <script>
      alert(1);
    </script>
    ```

- 我们可以使用一个 `<script>` 标签将 JavaScript 代码添加到页面中。

- `type` 和 `language` 特性（attribute）不是必需的。

- 外部的脚本可以通过 `<script src="path/to/script.js"></script>` 的方式插入。

### 1.2 代码结构

- **分号**

  - 当存在换行符（line break）时，在大多数情况下可以省略分号。JavaScript 将换行符理解成“隐式”的分号。这也被称为 [自动分号插入](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion)。

  - 在大多数情况下，换行意味着一个分号。但是“大多数情况”并不意味着“总是”！，例如：

    ```javascript
    alert(3 +
    1
    + 2);
    ```

    代码输出 `6`，因为 JavaScript 并没有在这里插入分号。显而易见的是，如果一行以加号 `"+"` 结尾，那么这是一个“不完整的表达式”，不需要分号。所以，这个例子得到了预期的结果。

  - 但存在 JavaScript 无法确定是否真的需要自动插入分号的情况。

    ```javascript
    alert("Hello")
    
    [1, 2].forEach(alert);
    
    //被视为alert("Hello")[1, 2].forEach(alert); 只有第一个 Hello 会被显示出来，并且有一个报错
    ```

- 注释

  - **不支持注释嵌套！**

    ```javascript
    不要在 /*...*/ 内嵌套另一个 /*...*/。
    下面这段代码报错而无法执行：
    
    /*
      /* 嵌套注释 ?!? */
    */
    alert( 'World' );
    ```

### 1.3 现代模式，“use strict”

ES5 规范增加了新的语言特性并且修改了一些已经存在的特性。为了保证旧的功能能够使用，大部分的修改是默认不生效的。你需要一个特殊的指令 —— `"use strict"` 来明确地激活这些特性。

当它处于脚本文件的顶部时，则整个脚本文件都将以“现代”模式进行工作。

比如：

```javascript
"use strict";

// 代码以现代模式工作
...
```

`"use strict"` 可以被放在函数体的开头。这样则可以只在该函数中启用严格模式。但通常人们会在整个脚本中启用严格模式。

- **确保 “use strict” 出现在最顶部**

  - 这里的严格模式就没有被启用：

  ```javascript
  alert("some code");
  // 下面的 "use strict" 会被忽略，必须在最顶部。
  
  "use strict";
  
  // 严格模式没有被激活
  ```

  - 只有注释可以出现在 `"use strict"` 的上面。

- **没有办法取消** `use strict`

- 现代 JavaScript 支持 “class” 和 “module” —— 高级语言结构（本教程后续章节会讲到），它们会自动启用 `use strict`。因此，如果我们使用它们，则无需添加 `"use strict"` 指令。

  **因此，目前我们欢迎将 `"use strict";` 写在脚本的顶部。稍后，当你的代码全都写在了 class 和 module 中时，你则可以将 `"use strict";` 这行代码省略掉。**

### 1.4 变量

- 我们可以使用 `var`、`let` 或 `const` 声明变量来存储数据。

  - `let` — 现代的变量声明方式。
  - `var` — 老旧的变量声明方式。一般情况下，我们不会再使用它。
  - `const` — 类似于 `let`，但是变量的值无法被修改。

  变量应当以一种容易理解变量内部是什么的方式进行命名。

- **未采用** `use strict` **下的赋值**

  - 一般，我们需要在使用一个变量前定义它。但是在早期，我们可以不使用 `let` 进行变量声明，而可以简单地通过赋值来创建一个变量。现在如果我们不在脚本中使用 `use strict` 声明启用严格模式，这仍然可以正常工作，这是为了保持对旧脚本的兼容。

    ```javascript
    // 注意：这个例子中没有 "use strict"
    
    num = 5; // 如果变量 "num" 不存在，就会被创建
    
    alert(num); // 5
    ```

  - 上面这是个糟糕的做法，严格模式下会报错。

    ```javascript
    "use strict";
    
    num = 5; // 错误：num 未定义
    ```

### 1.5 数据类型

JavaScript 中有八种基本的数据类型（译注：前七种为基本数据类型，也称为原始类型，而 `object` 为复杂数据类型）。

- `number` 用于任何类型的数字：整数或浮点数，在 `±(253-1)` 范围内的整数。

  - 除了常规的数字，还包括所谓的“特殊数值（“special numeric values”）”也属于这种类型：`Infinity`、`-Infinity` 和 `NaN`。

    - 我们可以通过除以 0 来得到它：

      ```javascript
      alert( 1 / 0 ); // Infinity
      ```

      或者在代码中直接使用它：

      ```javascript
      alert( Infinity ); // Infinity
      ```

    - `NaN` 代表一个计算错误。它是一个不正确的或者一个未定义的数学操作所得到的结果，比如：

      ```javascript
      alert( "not a number" / 2 ); // NaN，这样的除法是错误的
      ```

      `NaN` 是粘性的。任何对 `NaN` 的进一步数学运算都会返回 `NaN`：

      ```javascript
      alert( NaN + 1 ); // NaN
      alert( 3 * NaN ); // NaN
      alert( "not a number" / 2 - 1 ); // NaN
      ```

      所以，如果在数学表达式中有一个 `NaN`，会被传播到最终结果（只有一个例外：`NaN ** 0` （幂）结果为 `1`)

- `bigint` 用于任意长度的整数。

  - “number” 类型无法表示大于 `(2^53-1)`（即 `9007199254740991`），或小于 `-(2^53-1)` 的整数。

  - 可以通过将 `n` 附加到整数字段的末尾来创建 `BigInt` 值。

    ```javascript
    // 尾部的 "n" 表示这是一个 BigInt 类型
    const bigInt = 1234567890123456789012345678901234567890n;
    ```

- `string` 用于字符串：一个字符串可以包含 0 个或多个字符，所以没有单独的单字符类型。

  - JavaScript 中的字符串必须被括在引号里。

    ```javascript
    let str = "Hello";
    let str2 = 'Single quotes are ok too';
    let phrase = `can embed another ${str}`;
    ```

  - 在 JavaScript 中，有三种包含字符串的方式。

    1. 双引号：`"Hello"`.
    2. 单引号：`'Hello'`.
    3. 反引号：``Hello``.

  - 双引号和单引号都是“简单”引用，在 JavaScript 中两者几乎没有什么差别。

  - 反引号是 **功能扩展** 引号。它们允许我们通过将变量和表达式包装在 `${…}` 中，来将它们嵌入到字符串中。例如：

    ```javascript
    let name = "John";
    
    // 嵌入一个变量
    alert( `Hello, ${name}!` ); // Hello, John!
    
    // 嵌入一个表达式
    alert( `the result is ${1 + 2}` ); // the result is 3
    ```

  - **JavaScript 中没有** *character* **类型。**

- `boolean` 用于 `true` 和 `false`。

- `null` 用于未知的值 —— 只有一个 `null` 值的独立类型。

- `undefined` 用于未定义的值 —— 只有一个 `undefined` 值的独立类型。

  - 通常，使用 `null` 将一个“空”或者“未知”的值写入变量中，而 `undefined` 则保留作为未进行初始化的事物的默认初始值。

- `symbol` 用于唯一的标识符。

- `object` 用于更复杂的数据结构。

我们可以通过 `typeof` 运算符查看存储在变量中的数据类型。

- 通常用作 `typeof x`，但 `typeof(x)` 也可行。

- 以字符串的形式返回类型名称，例如 `"string"`。

- `typeof null` 会返回 `"object"` —— 这是 JavaScript 编程语言的一个错误，实际上它并不是一个 `object`。

  ```javascript
  typeof undefined // "undefined"
  
  typeof 0 // "number"
  
  typeof 10n // "bigint"
  
  typeof true // "boolean"
  
  typeof "foo" // "string"
  
  typeof Symbol("id") // "symbol"
  
  typeof Math // "object"  
  
  typeof null // "object" 
  
  typeof alert // "function"  
  ```

### 1.6 交互：alert、prompt、confirm

- `alert`

  显示信息。

  对 `alert` 的调用没有返回值。或者说返回的是 `undefined`。

- `prompt`

  显示信息要求用户输入文本。点击确定返回文本，点击取消或按下 Esc 键返回 `null`。以字符串形式返回

  举个例子：

  ```javascript
  let age = prompt('How old are you?', 100);
  
  alert(`You are ${age} years old!`); // You are 100 years old!
  ```

- `confirm`

  显示信息等待用户点击确定或取消。点击确定返回 `true`，点击取消或按下 Esc 键返回 `false`。

这些方法都是模态的：它们暂停脚本的执行，并且不允许用户与该页面的其余部分进行交互，直到窗口被解除。

上述所有方法共有两个限制：

1. 模态窗口的确切位置由浏览器决定。通常在页面中心。
2. 窗口的确切外观也取决于浏览器。我们不能修改它。

### 1.7 类型转换

有三种常用的类型转换：转换为 string 类型、转换为 number 类型和转换为 boolean 类型。

**字符串转换** —— 转换发生在输出内容`alert`的时候，也可以通过 `String(value)` 进行显式转换。原始类型值的 string 类型转换通常是很明显的。

**数字型转换** —— 转换发生在进行算术操作时，也可以通过 `Number(value)` 进行显式转换。

数字型转换遵循以下规则：

| 值             | 变成……                                                       |
| :------------- | :----------------------------------------------------------- |
| `undefined`    | `NaN`                                                        |
| `null`         | `0`                                                          |
| `true / false` | `1 / 0`                                                      |
| `string`       | “按原样读取”字符串，两端的空格会被忽略。空字符串变成 `0`。转换出错则输出 `NaN`。 |

**布尔型转换** —— 转换发生在进行逻辑操作时，也可以通过 `Boolean(value)` 进行显式转换。

布尔型转换遵循以下规则：

| 值                                    | 变成……  |
| :------------------------------------ | :------ |
| `0`, `null`, `undefined`, `NaN`, `""` | `false` |
| 其他值                                | `true`  |

上述的大多数规则都容易理解和记忆。人们通常会犯错误的值得注意的例子有以下几个：

- 对 `undefined` 进行数字型转换时，输出结果为 `NaN`，而非 `0`。
- 对 `"0"` 和只有空格的字符串（比如：`" "`）进行布尔型转换时，输出结果为 `true`。

### 1.8 基础运算符，数学

- `+  -  *  /  %  **` 

  ```javascript
  alert( 2 ** 2 ); // 2² = 4
  alert( 8 ** (1/3) ); // 2（1/3 次方与立方根相同)
  ```

- 用二元运算符 + 连接字符串

  ```javascript
  let s = "my" + "string";
  alert(s); // mystring
  ```

  注意：只要任意一个运算元是字符串，那么另一个运算元也将被转化为字符串。

  举个例子：

  ```javascript
  alert('1' + 2 ); // "12"
  alert(2 + '1' ); // "21"
  alert(2 + 2 + '1' ); // "41"，不是 "221"
  alert('1' + 2 + 2); // "122"，不是 "14"
  
  alert( 6 - '2' ); // 4，将 '2' 转换为数字
  alert( '6' / '2' ); // 3，将两个运算元都转换为数字
  ```

- 数字转化，一元运算符+

  一元运算符加号，或者说，加号 `+` 应用于单个值，对数字没有任何作用。但是如果运算元不是数字，加号 `+` 则会将其转化为数字。它的效果和 `Number(...)` 相同，但是更加简短。

  例如：

  ```javascript
  // 对数字无效
  let x = 1;
  alert( +x ); // 1
  
  // 转化非数字
  alert( +true ); // 1
  alert( +"" );   // 0
  
  let apples = "2";
  let oranges = "3";
  // 在二元运算符加号起作用之前，所有的值都被转化为了数字
  alert( +apples + +oranges ); // 5
  ```

- 运算符优先级

  - 一元运算符优先级高于二元运算符

- 赋值 = 返回一个值

  - 语句 `x = value` 将值 `value` 写入 `x` **然后返回 value**。

    ```javascript
    let a = 1;
    let b = 2;
    
    let c = 3 - (a = b + 1);
    
    alert( a ); // 3
    alert( c ); // 0
    ```

- 链式赋值

  ```javascript
  let a, b, c;
  
  a = b = c = 2 + 2;
  
  alert( a ); // 4
  alert( b ); // 4
  alert( c ); // 4
  ```

- 原地修改

  - `+=  -=  *=  /*`

- 自增/自减

- 位运算符

  - 按位与 ( `&` )
  - 按位或 ( `|` )
  - 按位异或 ( `^` )
  - 按位非 ( `~` )
  - 左移 ( `<<` )
  - 右移 ( `>>` )
  - 无符号右移 ( `>>>` )

- 逗号运算符

  - 逗号运算符能让我们处理多个语句，使用 `,` 将它们分开。每个语句都运行了，但是只有最后的语句的结果会被返回。

    举个例子：

    ```javascript
    let a = (1 + 2, 3 + 4);
    
    alert( a ); // 7（3 + 4 的结果）
    ```

### 1.9 值的比较

- 比较运算符始终返回布尔值。

- 字符串的比较，会按照“词典”顺序逐字符地比较大小。

- 当对不同类型的值进行比较时，它们会先被转化为数字（不包括严格相等检查）再进行比较。

  例如：

  ```javascript
  alert( '2' > 1 ); // true，字符串 '2' 会被转化为数字 2
  alert( '01' == 1 ); // true，字符串 '01' 会被转化为数字 1
  alert( true == 1 ); // true
  alert( false == 0 ); // true
  
  let a = 0;
  alert( Boolean(a) ); // false
  
  let b = "0";
  alert( Boolean(b) ); // true
  
  alert(a == b); // true!
  ```

- 在非严格相等 `==` 下，`null` 和 `undefined` 相等且各自不等于任何其他的值。

- 在使用 `> >=` 或 `< <=` 进行比较时，需要注意变量可能为 `null/undefined` 的情况。比较好的方法是单独检查变量是否等于 `null/undefined`。

  - `null/undefined` 会被转化为数字：`null` 被转化为 `0`，`undefined` 被转化为 `NaN`。
  
  - 通过比较 `null` 和 0 可得：
  
    ```javascript
    alert( null > 0 );  // (1) false
    alert( null == 0 ); // (2) false
    alert( null >= 0 ); // (3) true
    ```
  
  - 这是因为相等性检查 `==` 和普通比较符 `> < >= <=` 的代码逻辑是相互独立的。进行值的比较时，`null` 会被转化为数字，因此它被转化为了 `0`。这就是为什么（3）中 `null >= 0` 返回值是 true，（1）中 `null > 0` 返回值是 false。
  
    另一方面，`undefined` 和 `null` 在相等性检查 `==` 中不会进行任何的类型转换，它们有自己独立的比较规则，所以除了它们之间互等外，不会等于任何其他的值。这就解释了为什么（2）中 `null == 0` 会返回 false。

### 1.10 条件分支：if和'?'

- 数字 `0`、空字符串 `""`、`null`、`undefined` 和 `NaN` 都会被转换成 `false`。因为它们被称为“假值（falsy）”值。
- 其他值被转换为 `true`，所以它们被称为“真值（truthy）”

### 1.11 逻辑运算符

- `||`

  - **或运算寻找第一个真值**

    ```javascript
    result = value1 || value2 || value3;
    ```

    或运算符 `||` 做了如下的事情：

    - 从左到右依次计算操作数。
    - 处理每一个操作数时，都将其转化为布尔值。如果结果是 `true`，就停止计算，返回这个操作数的初始值。
    - 如果所有的操作数都被计算过（也就是，转换结果都是 `false`），则返回最后一个操作数。

    返回的值是操作数的初始形式，不会做布尔转换。

    换句话说，一个或运算 `||` 的链，将返回第一个真值，如果不存在真值，就返回该链的最后一个值。

    例如：

    ```javascript
    alert( 1 || 0 ); // 1（1 是真值）
    
    alert( null || 1 ); // 1（1 是第一个真值）
    alert( null || 0 || 1 ); // 1（第一个真值）
    
    alert( undefined || null || 0 ); // 0（都是假值，返回最后一个值）
    ```

  - **获取变量列表或者表达式中的第一个真值。**
  - **短路求值（Short-circuit evaluation）。**

- `&&`

  - **与运算寻找第一个假值**

    ```javascript
    result = value1 && value2 && value3;
    ```

    与运算 `&&` 做了如下的事：

    - 从左到右依次计算操作数。
    - 在处理每一个操作数时，都将其转化为布尔值。如果结果是 `false`，就停止计算，并返回这个操作数的初始值。
    - 如果所有的操作数都被计算过（例如都是真值），则返回最后一个操作数。

    换句话说，与运算返回第一个假值，如果没有假值就返回最后一个值。

    例如：

    ```javascript
    // 如果第一个操作数是真值，
    // 与运算返回第二个操作数：
    alert( 1 && 0 ); // 0
    alert( 1 && 5 ); // 5
    
    // 如果第一个操作数是假值，
    // 与运算将直接返回它。第二个操作数会被忽略
    alert( null && 5 ); // null
    alert( 0 && "no matter what" ); // 0
    ```

    我们也可以在一行代码上串联多个值。查看第一个假值是如何被返回的：

    ```javascript
    alert( 1 && 2 && null && 3 ); // null
    ```

    如果所有的值都是真值，最后一个值将会被返回：

    ```javascript
    alert( 1 && 2 && 3 ); // 3，最后一个值
    ```

- **与运算** `&&` **在或运算** `||` **之前进行**

  - 与运算 `&&` 的优先级比或运算 `||` 要高。

- **不要用** `||` **或** `&&` **来取代** `if`

- `!`（非）

  - 逻辑非运算符接受一个参数，并按如下运作：

    1. 将操作数转化为布尔类型：`true/false`。
    2. 返回相反的值。

  - 例如：

    ```javascript
    alert( !true ); // false
    alert( !0 ); // true
    ```

  - **两个非运算 `!!` 有时候用来将某个值转化为布尔类型：**

    ```javascript
    alert( !!"non-empty string" ); // true
    alert( !!null ); // false
    ```

    也就是，第一个非运算将该值转化为布尔类型并取反，第二个非运算再次取反。最后我们就得到了一个任意值到布尔值的转化。

    有更多详细的方法可以完成同样的事 —— 一个内建的 `Boolean` 函数：

    ```javascript
    alert( Boolean("non-empty string") ); // true
    alert( Boolean(null) ); // false
    ```

  - 非运算符 `!` 的优先级在所有逻辑运算符里面最高，所以它总是在 `&&` 和 `||` 之前执行。

### 1.12 空值合并运算符'??'

- 当一个值既不是 `null` 也不是 `undefined` 时，我们将其称为“已定义的（defined）”。

  - `a ?? b` 的结果是：

    - 如果 `a` 是已定义的，则结果为 `a`，
    - 如果 `a` 不是已定义的，则结果为 `b`。

  - 我们可以使用我们已知的运算符重写 `result = a ?? b`，像这样：

    ```javascript
    result = (a !== null && a !== undefined) ? a : b;
    ```

- `??` 的常见使用场景是提供默认值。

  例如，在这里，如果 `user` 的值不为 `null/undefined` 则显示 `user`，否则显示 `匿名`：

  ```javascript
  let user;
  
  alert(user ?? "匿名"); // 匿名（user 未定义）
  ```

  在下面这个例子中，我们将一个名字赋值给了 `user`：

  ```javascript
  let user = "John";
  
  alert(user ?? "匿名"); // John（user 已定义）
  ```

- 我们还可以使用 `??` 序列从一系列的值中选择出第一个非 `null/undefined` 的值。

  ```javascript
  let firstName = null;
  let lastName = null;
  let nickName = "Supercoder";
  
  // 显示第一个已定义的值：
  alert(firstName ?? lastName ?? nickName ?? "匿名"); // Supercoder
  ```

- **与`||`比较**

  - 它们之间重要的区别是：
    - `||` 返回第一个 **真** 值。
    - `??` 返回第一个 **已定义的** 值。
  - 换句话说，`||` 无法区分 `false`、`0`、空字符串 `""` 和 `null/undefined`。它们都一样 —— 假值（falsy values）。如果其中任何一个是 `||` 的第一个参数，那么我们将得到第二个参数作为结果。

  - `??` 运算符的优先级与 `||` 相同
    - 这意味着，就像 `||` 一样，空值合并运算符在 `=` 和 `?` 运算前计算，但在大多数其他运算（例如 `+` 和 `*`）之后计算。
  - 出于安全原因，JavaScript 禁止将 `??` 运算符与 `&&` 和 `||` 运算符一起使用，除非使用括号明确指定了优先级。

### 1.13 循环：while和for

- **禁止** `break/continue` **在 ‘?’ 的右边**

  - 例如，我们使用如下代码：

    ```javascript
    if (i > 5) {
      alert(i);
    } else {
      continue;
    }
    ```

    ……用问号重写：

    ```javascript
    (i > 5) ? alert(i) : continue; // continue 不允许在这个位置
    ```

    ……代码会停止运行，并显示有语法错误。

- **break/continue标签**

  - **标签** 是在循环之前带有冒号的标识符：

    ```javascript
    labelName: for (...) {
      ...
    }
    ```

  - `break <labelName>` 语句跳出循环至标签处：

    ```javascript
    outer: for (let i = 0; i < 3; i++) {
    
      for (let j = 0; j < 3; j++) {
    
        let input = prompt(`Value at coords (${i},${j})`, '');
    
        // 如果是空字符串或被取消，则中断并跳出这两个循环。
        if (!input) break outer; // (*)
    
        // 用得到的值做些事……
      }
    }
    
    alert('Done!');
    ```

    我们还可以将标签移至单独一行：

    ```javascript
    outer:
    for (let i = 0; i < 3; i++) { ... }
    ```
    
    `continue` 指令也可以与标签一起使用。在这种情况下，执行跳转到标记循环的下一次迭代。

- **标签并不允许“跳到”所有位置**

  例如，这样做是不可能的：

  ```javascript
  break label;  // 跳转至下面的 label 处（无效）
  
  label: for (...)
  ```

  `break` 指令必须在代码块内。从技术上讲，任何被标记的代码块都有效，例如：

  ```javascript
  label: {
    // ...
    break label; // 有效
    // ...
  }
  ```

  `continue` 只有在循环内部才可行。

### 1.14 switch语句

- `switch/case` 有通过 case 进行“分组”的能力，其实是 switch 语句没有 `break` 时的副作用。因为没有 `break`，`case 3` 会从 `(*)` 行执行到 `case 5`。

  ```javascript
  let a = 3;
  
  switch (a) {
    case 4:
      alert('Right!');
      break;
  
    case 3: // (*) 下面这两个 case 被分在一组
    case 5:
      alert('Wrong!');
      alert("Why don't you take a math class?");
      break;
  
    default:
      alert('The result is strange. Really.');
  }
  ```

- 这里的相等是**严格相等**。被比较的值必须是相同的类型才能进行匹配。

### 1.15 函数

函数声明方式如下所示：

```javascript
function name(parameters, delimited, by, comma) {
  /* code */
}
```

- 作为参数传递给函数的值，会被复制到函数的局部变量。
- 函数可以访问外部变量。但它只能从内到外起作用。函数外部的代码看不到函数内的局部变量。
- 函数可以返回值。如果没有返回值或为空值，则其返回的结果是 `undefined`。
- 如果一个函数被调用，但有参数（argument）未被提供，那么相应的值就会变成 `undefined`。
- 我们可以使用 `=` 为函数声明中的参数指定所谓的“默认”（如果对应参数的值未被传递则使用）值：

**不要在** `return` **与返回值之间添加新行**

-  JavaScript 默认会在 `return` 之后加上分号。

为了使代码简洁易懂，建议在函数中主要使用局部变量和参数，而不是外部变量。

与不获取参数但将修改外部变量作为副作用的函数相比，获取参数、使用参数并返回结果的函数更容易理解。

函数命名：

- 函数名应该清楚地描述函数的功能。当我们在代码中看到一个函数调用时，一个好的函数名能够让我们马上知道这个函数的功能是什么，会返回什么。
- 一个函数是一个行为，所以函数名通常是动词。
- 目前有许多优秀的函数名前缀，如 `create…`、`show…`、`get…`、`check…` 等等。使用它们来提示函数的作用吧。

### 1.16 函数表达式

- 另一种创建函数的语法称为 **函数表达式**。

  它允许我们在任何表达式的中间创建一个新函数。

  例如：

  ```javascript
  function sayHi() {
    alert( "Hello" );
  }
  
  let sayHi = function() {
    alert( "Hello" );
  };
  ```

  由于函数创建发生在赋值表达式的上下文中（在 `=` 的右侧），因此这是一个 **函数表达式**。

  `function` 关键字后面没有函数名。函数表达式允许省略函数名。

- **函数是值**。它们可以在代码的任何地方被分配，复制或声明。

  - 我们还可以用 `alert` 显示这个变量的值：

    ```javascript
    function sayHi() {
      alert( "Hello" );
    }
    
    alert( sayHi ); // 显示函数源代码
    ```

  - 我们可以复制函数到其他变量：

    ```javascript
    function sayHi() {   // (1) 创建
      alert( "Hello" );
    }
    
    let func = sayHi;    // (2) 复制
    
    func(); // Hello     // (3) 运行复制的值（正常运行）！
    sayHi(); // Hello    //     这里也能运行
    ```

- **回调函数**

  - 函数需要提出 `question`（问题），并根据用户的回答，调用 `yes()` 或 `no()`：

    ```javascript
    function ask(question, yes, no) {
      if (confirm(question)) yes()
      else no();
    }
    
    function showOk() {
      alert( "You agreed." );
    }
    
    function showCancel() {
      alert( "You canceled the execution." );
    }
    
    // 用法：函数 showOk 和 showCancel 被作为参数传入到 ask
    ask("Do you agree?", showOk, showCancel);
    ```

  - `ask` 的两个参数值 `showOk` 和 `showCancel` 可以被称为 **回调函数** 或简称 **回调**。

- **匿名函数**

  - 我们可以使用函数表达式来编写一个等价的、更简洁的函数：

    ```javascript
    function ask(question, yes, no) {
      if (confirm(question)) yes()
      else no();
    }
    
    ask(
      "Do you agree?",
      function() { alert("You agreed."); },
      function() { alert("You canceled the execution."); }
    );
    ```

    这里直接在 `ask(...)` 调用内进行函数声明。这两个函数没有名字，所以叫 **匿名函数**。这样的函数在 `ask` 外无法访问（因为没有对它们分配变量）

- 如果函数在主代码流中被声明为单独的语句，则称为“函数声明”。

- 如果该函数是作为表达式的一部分创建的，则称其“函数表达式”。

- 在执行代码块之前，内部算法会**先处理函数声明**。所以函数声明在其被声明的代码块内的任何位置都是可见的。

- 函数表达式在执行流程到达时创建。

- **严格模式下，当一个函数声明在一个代码块内时，它在该代码块内的任何位置都是可见的。但在代码块外不可见。**

  ```javascript
  let age = 16; // 拿 16 作为例子
  
  if (age < 18) {
    welcome();               // \   (运行)
                             //  |
    function welcome() {     //  |
      alert("Hello!");       //  |  函数声明在声明它的代码块内任意位置都可用
    }                        //  |
                             //  |
    welcome();               // /   (运行)
  
  } else {
  
    function welcome() {
      alert("Greetings!");
    }
  }
  
  // 在这里，我们在花括号外部调用函数，我们看不到它们内部的函数声明。
  
  
  welcome(); // Error: welcome is not defined
  ```

  下面的代码可以如愿运行：

  ```javascript
  let age = prompt("What is your age?", 18);
  
  let welcome;
  
  if (age < 18) {
  
    welcome = function() {
      alert("Hello!");
    };
  
  } else {
  
    welcome = function() {
      alert("Greetings!");
    };
  
  }
  
  welcome(); // 现在可以了
  ```

  或者我们可以使用问号运算符 `?` 来进一步对代码进行简化：

  ```javascript
  let age = prompt("What is your age?", 18);
  
  let welcome = (age < 18) ?
    function() { alert("Hello!"); } :
    function() { alert("Greetings!"); };
  
  welcome(); // 现在可以了
  ```

在大多数情况下，当我们需要声明一个函数时，最好使用函数声明，因为函数在被声明之前也是可见的。这使我们在代码组织方面更具灵活性，通常也会使得代码可读性更高。

### 1.17 箭头函数，基础知识

### 1.18 JavaScript特性

## 2. 代码质量
