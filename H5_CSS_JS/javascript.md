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

创建函数还有另外一种非常简单的语法，并且这种方法通常比函数表达式更好。它被称为“箭头函数”

```javascript
let func = (arg1, arg2, ..., argN) => expression;
```

这里创建了一个函数 `func`，它接受参数 `arg1..argN`，然后使用参数对右侧的 `expression` 求值并返回其结果。

换句话说，它是下面这段代码的更短的版本：

```javascript
let func = function(arg1, arg2, ..., argN) {
  return expression;
};
```

如果没有参数，括号则是空的（但括号必须保留）

例如，动态创建一个函数：

```javascript
let age = prompt("What is your age?", 18);

let welcome = (age < 18) ?
  () => alert('Hello!') :
  () => alert("Greetings!");

welcome();
```

- 多行的箭头函数

  有时我们需要更复杂一点的函数，比如带有多行的表达式或语句。在这种情况下，我们可以使用花括号将它们括起来。主要区别在于，用花括号括起来之后，需要包含 `return` 才能返回值（就像常规函数一样）。

  ```javascript
  let sum = (a, b) => {  // 花括号表示开始一个多行函数
    let result = a + b;
    return result; // 如果我们使用了花括号，那么我们需要一个显式的 “return”
  };
  
  alert( sum(1, 2) ); // 3
  ```

### 1.18 JavaScript特性

## 2. 代码质量

### 2.1 在浏览器中调试

- 有 3 种方式来暂停一个脚本：

  1. 断点。
  2. `debugger` 语句。
  3. error（如果开发者工具是打开状态，并且按钮 是开启的状态）。

- 日志记录

  - `console.log` 函数

    例如：将从 `0` 到 `4` 的值输出到控制台上：

    ```javascript
    // 打开控制台来查看
    for (let i = 0; i < 5; i++) {
      console.log("value", i);
    }
    ```

### 2.2 代码风格

没有人喜欢读一长串代码，最好将代码分割一下。

例如：

```javascript
// 回勾引号 ` 允许将字符串拆分为多行
let str = `
  ECMA International's TC39 is a group of JavaScript developers,
  implementers, academics, and more, collaborating with the community
  to maintain and evolve the definition of JavaScript.
`;
```

### 2.3 注释

一个好的开发者的标志之一就是他的注释：它们的存在甚至它们的缺席（译注：在该注释的地方注释，在不需要注释的地方则不注释，甚至写得好的自描述函数本身就是一种注释）。

好的注释可以使我们更好地维护代码，一段时间之后依然可以更高效地回到代码高效开发。

**注释这些内容：**

- 整体架构，高层次的观点。
- 函数的用法。
- 重要的解决方案，特别是在不是很明显时。

**避免注释：**

- 描述“代码如何工作”和“代码做了什么”。
- 避免在代码已经足够简单或代码有很好的自描述性而不需要注释的情况下，还写些没必要的注释。

### 2.4 忍者代码

### 2.5 使用Mocha进行自动化测试

### 2.6 Polyfill和转译器

- 转译器是一种可以将源码转译成另一种源码的特殊的软件。它可以解析（“阅读和理解”）现代代码，并使用旧的语法结构对其进行重写，进而使其也可以在旧的引擎中工作。
- 更新/添加新函数的脚本被称为“polyfill”。它“填补”了空白并添加了缺失的实现。

## 3. Object（对象）：基础知识

### 3.1 对象

- 可以用下面两种语法中的任一种来创建一个空的对象：

  ```javascript
  let user = new Object(); // “构造函数” 的语法
  let user = {};  // “字面量” 的语法
  
  let user = {     // 一个对象
    name: "John",  // 键 "name"，值 "John"
    age: 30        // 键 "age"，值 30
  };
  ```

- 可以使用点符号访问属性值：

  ```javascript
  // 读取文件的属性：
  alert( user.name ); // John
  alert( user.age ); // 30
  ```

- 属性的值可以是任意类型，如添加一个布尔类型：

  ```javascript
  user.isAdmin = true;
  ```

- 可以用 `delete` 操作符移除属性：

  ```javascript
  delete user.age;
  ```

- 也可以用多字词语来作为属性名，但必须给它们加上引号：

  ```javascript
  let user = {
    name: "John",
    age: 30,
    "likes birds": true  // 多词属性名必须加引号
  };
  ```

- 对于多词属性，点操作就不能用了，另一种方法，就是使用方括号，可用于任何字符串：

  ```javascript
  let user = {};
  
  // 设置
  user["likes birds"] = true;
  
  // 读取
  alert(user["likes birds"]); // true
  
  // 删除
  delete user["likes birds"];
  ```

- 方括号同样提供了一种可以通过任意表达式来获取属性名的方式：

  ```javascript
  let key = "likes birds";
  
  // 跟 user["likes birds"] = true; 一样
  user[key] = true;
  ```

  在这里，变量 `key` 可以是程序运行时计算得到的，也可以是根据用户的输入得到的。然后我们可以用它来访问属性。这给了我们很大的灵活性。

- 当创建一个对象时，我们可以在对象字面量中使用方括号。这叫做 **计算属性**。

  例如：

  ```javascript
  let fruit = prompt("Which fruit to buy?", "apple");
  
  let bag = {
    [fruit]: 5, // 属性名是从 fruit 变量中得到的
  };
  
  alert( bag.apple ); // 5 如果 fruit="apple"
  ```

- 我们可以在方括号中使用更复杂的表达式：

  ```javascript
  let fruit = 'apple';
  let bag = {
    [fruit + 'Computers']: 5 // bag.appleComputers = 5
  };
  ```

  - 方括号比点符号更强大。它允许任何属性名和变量，但写起来也更加麻烦。

  - 所以，大部分时间里，当属性名是已知且简单的时候，就使用点符号。如果我们需要一些更复杂的内容，那么就用方括号。

- 属性值简写

  - 例如：

    ```javascript
    function makeUser(name, age) {
      return {
        name: name,
        age: age,
        // ……其他的属性
      };
    }
    
    let user = makeUser("John", 30);
    alert(user.name); // John
    ```

    在上面的例子中，属性名跟变量名一样。这种通过变量生成属性的应用场景很常见，在这有一种特殊的 **属性值缩写** 方法，使属性名变得更短。

    可以用 `name` 来代替 `name:name` 像下面那样：

    ```javascript
    function makeUser(name, age) {
      return {
        name, // 与 name: name 相同
        age,  // 与 age: age 相同
        // ...
      };
    }
    ```

    我们可以把属性名简写方式和正常方式混用：

    ```javascript
    let user = {
      name,  // 与 name:name 相同
      age: 30
    };
    ```

- 属性命名没有限制。属性名可以是任何字符串或者 symbol

  - 其他类型会被自动地转换为字符串。

    ```javascript
    let obj = {
      0: "test" // 等同于 "0": "test"
    };
    
    // 都会输出相同的属性（数字 0 被转为字符串 "0"）
    alert( obj["0"] ); // test
    alert( obj[0] ); // test (相同的属性)
    ```

  - 这里有个小陷阱：一个名为 `__proto__` 的属性。我们不能将它设置为一个非对象的值：

    ```javascript
    let obj = {};
    obj.__proto__ = 5; // 分配一个数字
    alert(obj.__proto__); // [object Object] —— 值为对象，与预期结果不同
    ```

- 属性存在性测试，"in"操作符

  - 相比于其他语言，JavaScript 的对象有一个需要注意的特性：能够被访问任何属性。即使属性不存在也不会报错！

    读取不存在的属性只会得到 `undefined`。所以我们可以很容易地判断一个属性是否存在：

    ```javascript
    let user = {};
    
    alert( user.noSuchProperty === undefined ); // true 意思是没有这个属性
    ```

  - 检查属性是否存在的操作符 `"in"`

    例如：

    ```javascript
    let user = { name: "John", age: 30 };
    
    alert( "age" in user ); // true，user.age 存在
    alert( "blabla" in user ); // false，user.blabla 不存在。
    ```

  - 请注意，`in` 的左边必须是 **属性名**。通常是一个带引号的字符串。

    如果我们省略引号，就意味着左边是一个变量，它应该包含要判断的实际属性名。例如：

    ```javascript
    let user = { age: 30 };
    
    let key = "age";
    alert( key in user ); // true，属性 "age" 存在
    ```

  - 为何会有 `in` 运算符呢？

    属性存在，但存储的值是 `undefined` 的时候：

    ```javascript
    let obj = {
      test: undefined
    };
    
    alert( obj.test ); // 显示 undefined，所以属性不存在？
    
    alert( "test" in obj ); // true，属性存在！
    ```

- **"for..in" 循环**

  - 为了遍历一个对象的所有键（key），可以使用一个特殊形式的循环：`for..in`。

    例如，让我们列出 `user` 所有的属性：

    ```javascript
    let user = {
      name: "John",
      age: 30,
      isAdmin: true
    };
    
    for (let key in user) {
      // keys
      alert( key );  // name, age, isAdmin
      // 属性键的值
      alert( user[key] ); // John, 30, true
    }
    ```

- 排序

  - 整数属性会被进行排序，其他属性则按照创建的顺序显示。详情如下：

    例如，让我们考虑一个带有电话号码的对象：

    ```javascript
    let codes = {
      "49": "Germany",
      "41": "Switzerland",
      "44": "Great Britain",
      // ..,
      "1": "USA"
    };
    
    for(let code in codes) {
      alert(code); // 1, 41, 44, 49
    }
    ```

### 3.2 对象引用和复制

- 对象通过引用被赋值和拷贝。换句话说，一个变量存储的不是“对象的值”，而是一个对值的“引用”（内存地址）。因此，拷贝此类变量或将其作为函数参数传递时，所拷贝的是引用，而不是对象本身。

- 所有通过被拷贝的引用的操作（如添加、删除属性）都作用在同一个对象上。

- 仅当两个对象为同一对象时，两者才相等。`==`，`===`

- 我们可以创建一个新对象，通过遍历已有对象的属性，并在原始类型值的层面复制它们，以实现对已有对象结构的复制。

  就像这样：

  ```javascript
  let user = {
    name: "John",
    age: 30
  };
  
  let clone = {}; // 新的空对象
  
  // 将 user 中所有的属性拷贝到其中
  for (let key in user) {
    clone[key] = user[key];
  }
  
  // 现在 clone 是带有相同内容的完全独立的对象
  clone.name = "Pete"; // 改变了其中的数据
  
  alert( user.name ); // 原来的对象中的 name 属性依然是 John
  ```

  我们也可以使用 Object.assign 方法来达成同样的效果。

- 为了创建“真正的拷贝”（一个克隆），我们可以使用 `Object.assign` 来做所谓的“浅拷贝”（嵌套对象被通过引用进行拷贝）或者使用“深拷贝”函数，例如 [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep)。

  - 语法是：

    ```javascript
    Object.assign(dest, [src1, src2, src3...])
    ```

    - 第一个参数 `dest` 是指目标对象。
    - 更后面的参数 `src1, ..., srcN`（可按需传递多个参数）是源对象。
    - 该方法将所有源对象的属性拷贝到目标对象 `dest` 中。换句话说，从第二个开始的所有参数的属性都被拷贝到第一个参数的对象中。
    - 调用结果返回 `dest`。

  - 我们可以用它来合并多个对象：

    ```javascript
    let user = { name: "John" };
    
    let permissions1 = { canView: true };
    let permissions2 = { canEdit: true };
    
    // 将 permissions1 和 permissions2 中的所有属性都拷贝到 user 中
    Object.assign(user, permissions1, permissions2);
    
    // 现在 user = { name: "John", canView: true, canEdit: true }
    ```

  - 如果被拷贝的属性的属性名已经存在，那么它会被覆盖：

    ```javascript
    let user = { name: "John" };
    
    Object.assign(user, { name: "Pete" });
    
    alert(user.name); // 现在 user = { name: "Pete" }
    ```

    我们也可以用 `Object.assign` 代替 `for..in` 循环来进行简单克隆：

    ```javascript
    let user = {
      name: "John",
      age: 30
    };
    
    let clone = Object.assign({}, user);
    ```

    它将 `user` 中的所有属性拷贝到了一个空对象中，并返回这个新的对象。

- 深层克隆

  - 属性可以是对其他对象的引用。
  - 为了解决这个问题，我们应该使用一个拷贝循环来检查 `user[key]` 的每个值，如果它是一个对象，那么也复制它的结构。这就是所谓的“深拷贝”。

- **使用 const 声明的对象也是可以被修改的**

  通过引用对对象进行存储的一个重要的副作用是声明为 `const` 的对象 **可以** 被修改。

  例如：

  ```javascript
  const user = {
    name: "John"
  };
  
  user.name = "Pete"; // (*)
  
  alert(user.name); // Pete
  ```

  看起来 `(*)` 行的代码会触发一个错误，但实际并没有。`user` 的值是一个常量，它必须始终引用同一个对象，但该对象的属性可以被自由修改。

  换句话说，只有当我们尝试将 `user=...` 作为一个整体进行赋值时，`const user` 才会报错。

### 3.3 垃圾回收

主要需要掌握的内容：

- 垃圾回收是自动完成的，我们不能强制执行或是阻止执行。
- 当对象是可达状态时，它一定是存在于内存中的。
- 被引用与可访问（从一个根）不同：一组相互连接的对象可能整体都不可达，正如我们在上面的例子中看到的那样。

现代引擎实现了垃圾回收的高级算法。

### 3.4 对象方法，"this"

- 方法示例

  ```javascript
  let user = {
    name: "John",
    age: 30
  };
  
  user.sayHi = function() {
    alert("Hello!");
  };
  
  user.sayHi(); // Hello!
  ```

  - 我们也可以使用预先声明的函数作为方法，就像这样：

  ```javascript
  let user = {
    // ...
  };
  
  // 首先，声明函数
  function sayHi() {
    alert("Hello!");
  }
  
  // 然后将其作为一个方法添加
  user.sayHi = sayHi;
  
  user.sayHi(); // Hello!
  ```

- 方法简写

  ```javascript
  // 这些对象作用一样
  
  user = {
    sayHi: function() {
      alert("Hello");
    }
  };
  
  // 方法简写看起来更好，对吧？
  let user = {
    sayHi() { // 与 "sayHi: function(){...}" 一样
      alert("Hello");
    }
  };
  ```

- **"this" 不受限制**

  JavaScript 中的 `this` 可以用于任何函数，即使它不是对象的方法。

  下面这样的代码没有语法错误：

  ```javascript
  function sayHi() {
    alert( this.name );
  }
  ```

  `this` 的值是在代码运行时计算出来的，它取决于代码上下文。

  例如，这里相同的函数被分配给两个不同的对象，在调用中有着不同的 “this” 值：

  ```javascript
  let user = { name: "John" };
  let admin = { name: "Admin" };
  
  function sayHi() {
    alert( this.name );
  }
  
  // 在两个对象中使用相同的函数
  user.f = sayHi;
  admin.f = sayHi;
  
  // 这两个调用有不同的 this 值
  // 函数内部的 "this" 是“点符号前面”的那个对象
  user.f(); // John（this == user）
  admin.f(); // Admin（this == admin）
  
  admin['f'](); // Admin（使用点符号或方括号语法来访问这个方法，都没有关系。）
  ```

  这个规则很简单：如果 `obj.f()` 被调用了，则 `this` 在 `f` 函数调用期间是 `obj`。所以在上面的例子中 this 先是 `user`，之后是 `admin`。

- **在没有对象的情况下调用：**`this == undefined`

- **箭头函数没有自己的"this"**

  - 箭头函数有些特别：它们没有自己的 `this`。如果我们在这样的函数中引用 `this`，`this` 值取决于外部“正常的”函数。

    举个例子，这里的 `arrow()` 使用的 `this` 来自于外部的 `user.sayHi()` 方法：

    ```javascript
    let user = {
      firstName: "Ilya",
      sayHi() {
        let arrow = () => alert(this.firstName);
        arrow();
      }
    };
    
    user.sayHi(); // Ilya
    ```

### 3.5 构造器和操作符 "new"

- 构造函数，或简称构造器，就是常规函数，但大家对于构造器有个共同的约定，就是其命名首字母要大写。

  例如：

  ```javascript
  function User(name) {
    this.name = name;
    this.isAdmin = false;
  }
  
  let user = new User("Jack");
  
  alert(user.name); // Jack
  alert(user.isAdmin); // false
  ```

- 当一个函数被使用 `new` 操作符执行时，它按照以下步骤：

  1. 一个新的空对象被创建并分配给 `this`。
  2. 函数体执行。通常它会修改 `this`，为其添加新的属性。
  3. 返回 `this` 的值。

- 构造函数只能使用 `new` 来调用。这样的调用意味着在开始时创建了空的 `this`，并在最后返回填充了值的 `this`。

- 从技术上讲，任何函数（除了箭头函数，它没有自己的 `this`）都可以用作构造器。即可以通过 `new` 来运行，它会执行上面的算法。“首字母大写”是一个共同的约定，以明确表示一个函数将被使用 `new` 来运行。

- **new function() { … }**

  如果我们有许多行用于创建单个复杂对象的代码，我们可以将它们封装在一个立即调用的构造函数中，像这样：

  ```javascript
  // 创建一个函数并立即使用 new 调用它
  let user = new function() {
    this.name = "John";
    this.isAdmin = false;
  
    // ……用于用户创建的其他代码
    // 也许是复杂的逻辑和语句
    // 局部变量等
  };
  ```

  这个构造函数不能被再次调用，因为它不保存在任何地方，只是被创建和调用。因此，这个技巧旨在封装构建单个对象的代码，而无需将来重用。

- 构造器的return

  通常，构造器没有 `return` 语句。它们的任务是将所有必要的东西写入 `this`，并自动转换为结果。

  但是，如果这有一个 `return` 语句，那么规则就简单了：

  - 如果 `return` 返回的是一个对象，则返回这个对象，而不是 `this`。
  - 如果 `return` 返回的是一个原始类型或空，则忽略。

- **省略括号**

  如果没有参数，我们可以省略 `new` 后的括号：

  ```javascript
  let user = new User; // <-- 没有参数
  // 等同于
  let user = new User();
  ```

- 构造器中的方法

  例如，下面的 `new User(name)` 用给定的 `name` 和方法 `sayHi` 创建了一个对象：

  ```javascript
  function User(name) {
    this.name = name;
  
    this.sayHi = function() {
      alert( "My name is: " + this.name );
    };
  }
  
  let john = new User("John");
  
  john.sayHi(); // My name is: John
  
  /*
  john = {
     name: "John",
     sayHi: function() { ... }
  }
  */
  ```

### 3.6 可选链 "?."

- 可选链 `?.` 是一种访问嵌套对象属性的安全的方式。即使中间的属性不存在，也不会出现错误。

- 如果可选链 `?.` 前面的值为 `undefined` 或者 `null`，它会停止运算并返回 `undefined`。

  下面这是一种使用 `?.` 安全地访问 `user.address.street` 的方式：

  ```javascript
  let user = {}; // user 没有 address 属性
  
  alert( user?.address?.street ); // undefined（不报错）
  ```

- **不要过度使用可选链**
  - 我们应该只将 `?.` 使用在一些东西可以不存在的地方。
  - 例如，如果根据我们的代码逻辑，`user` 对象必须存在，但 `address` 是可选的，那么我们应该这样写 `user.address?.street`，而不是这样 `user?.address?.street`。
  - 那么，如果 `user` 恰巧为 undefined，我们会看到一个编程错误并修复它。否则，如果我们滥用 `?.`，会导致代码中的错误在不应该被消除的地方消除了，这会导致调试更加困难。

- `?.` **前的变量必须已声明**

- 如果 `?.` 左边部分不存在，就会立即停止运算（“短路效应”）。

  因此，如果在 `?.` 的右侧有任何进一步的函数调用或操作，它们均不会执行。

- 其他变体：?.()，?.[]

  - 将 `?.()` 用于调用一个可能不存在的函数。

    在下面这段代码中，有些用户具有 `admin` 方法，而有些没有：

    ```javascript
    let userAdmin = {
      admin() {
        alert("I am admin");
      }
    };
    
    let userGuest = {};
    
    userAdmin.admin?.(); // I am admin
    
    userGuest.admin?.(); // 啥都没发生（没有这样的方法）
    ```

  - 如果我们想使用方括号 `[]` 而不是点符号 `.` 来访问属性，语法 `?.[]` 也可以使用。跟前面的例子类似，它允许从一个可能不存在的对象上安全地读取属性。

    ```javascript
    let key = "firstName";
    
    let user1 = {
      firstName: "John"
    };
    
    let user2 = null;
    
    alert( user1?.[key] ); // John
    alert( user2?.[key] ); // undefined
    ```

- **我们可以使用** `?.` **来安全地读取或删除，但不能写入**

  - 可选链 `?.` 不能用在赋值语句的左侧。

    例如：

    ```javascript
    let user = null;
    
    user?.name = "John"; // Error，不起作用
    // 因为它在计算的是：undefined = "John"
    ```

- 可选链 `?.` 语法有三种形式：
  1. `obj?.prop` —— 如果 `obj` 存在则返回 `obj.prop`，否则返回 `undefined`。
  2. `obj?.[prop]` —— 如果 `obj` 存在则返回 `obj[prop]`，否则返回 `undefined`。
  3. `obj.method?.()` —— 如果 `obj.method` 存在则调用 `obj.method()`，否则返回 `undefined`。

### 3.7 symbol类型

- 根据规范，只有两种原始类型可以用作对象属性键：

  - 字符串类型

  - symbol 类型

  否则，如果使用另一种类型，例如数字，它会被自动转换为字符串。所以 `obj[1]` 与 `obj["1"]` 相同，而 `obj[true]` 与 `obj["true"]` 相同。

- `symbol` 是唯一标识符的基本类型

- symbol 是使用带有可选描述（name）的 `Symbol()` 调用创建的。

  创建时，我们可以给 symbol 一个描述（也称为 symbol 名），这在代码调试时非常有用：

  ```javascript
  // id 是描述为 "id" 的 symbol
  let id = Symbol("id");
  
  let id = Symbol();
  ```

- symbol 总是不同的值，即使它们有相同的名字。如果我们希望同名的 symbol 相等，那么我们应该使用全局注册表：`Symbol.for(key)` 返回（如果需要的话则创建）一个以 `key` 作为名字的全局 symbol。使用 `Symbol.for` 多次调用 `key` 相同的 symbol 时，返回的就是同一个 symbol。

- **symbol 不会被自动转换为字符串**

  - 例如，这个 `alert` 将会提示出错：

    ```javascript
    let id = Symbol("id");
    alert(id); // 类型错误：无法将 symbol 值转换为字符串。
    ```

    - 这是一种防止混乱的“语言保护”，因为字符串和 symbol 有本质上的不同，不应该意外地将它们转换成另一个。

  - 如果我们真的想显示一个 symbol，我们需要在它上面调用 `.toString()`，如下所示：

    ```javascript
    let id = Symbol("id");
    alert(id.toString()); // Symbol(id)，现在它有效了
    ```

    或者获取 `symbol.description` 属性，只显示描述（description）：

    ```javascript
    let id = Symbol("id");
    alert(id.description); // id
    ```

- symbol 有两个主要的使用场景：

  1. “隐藏” 对象属性。

     - 如果我们想要向“属于”另一个脚本或者库的对象添加一个属性，我们可以创建一个 symbol 并使用它作为属性的键。symbol 属性不会出现在 `for..in` 中，因此它不会意外地被与其他属性一起处理。并且，它不会被直接访问，因为另一个脚本没有我们的 symbol。因此，该属性将受到保护，防止被意外使用或重写。

     - 因此我们可以使用 symbol 属性“秘密地”将一些东西隐藏到我们需要的对象中，但其他地方看不到它。
     - 但如果我们处于同样的目的，使用字符串 `"id"` 而不是用 symbol，那么 **就会** 出现冲突：

  1. JavaScript 使用了许多系统 symbol，这些 symbol 可以作为 `Symbol.*` 访问。我们可以使用它们来改变一些内建行为。例如，在本教程的后面部分，我们将使用 `Symbol.iterator` 来进行 [迭代](https://zh.javascript.info/iterable) 操作，使用 `Symbol.toPrimitive` 来设置 [对象原始值的转换](https://zh.javascript.info/object-toprimitive) 等等。

从技术上说，symbol 不是 100% 隐藏的。有一个内建方法 [Object.getOwnPropertySymbols(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) 允许我们获取所有的 symbol。还有一个名为 [Reflect.ownKeys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) 的方法可以返回一个对象的 **所有** 键，包括 symbol。但大多数库、内建方法和语法结构都没有使用这些方法。

- 对象字面量中的symbol

  - 如果我们要在对象字面量 `{...}` 中使用 symbol，则需要使用方括号把它括起来。

    就像这样：

    ```javascript
    let id = Symbol("id");
    
    let user = {
      name: "John",
      [id]: 123 // 而不是 "id"：123
    };
    ```

    这是因为我们需要变量 `id` 的值作为键，而不是字符串 “id”。

- 全局symbol

  - 这里有一个 **全局 symbol 注册表**。我们可以在其中创建 symbol 并在稍后访问它们，它可以确保每次访问相同名字的 symbol 时，返回的都是相同的 symbol。

  - 要从注册表中读取（不存在则创建）symbol，请使用 `Symbol.for(key)`。

  - 该调用会检查全局注册表，如果有一个描述为 `key` 的 symbol，则返回该 symbol，否则将创建一个新 symbol（`Symbol(key)`），并通过给定的 `key` 将其存储在注册表中。

  - 例如：

    ```javascript
    // 从全局注册表中读取
    let id = Symbol.for("id"); // 如果该 symbol 不存在，则创建它
    
    // 再次读取（可能是在代码中的另一个位置）
    let idAgain = Symbol.for("id");
    
    // 相同的 symbol
    alert( id === idAgain ); // true
    ```

- 对于全局 symbol，`Symbol.for(key)` 按名字返回一个 symbol。相反，通过全局 symbol 返回一个名字，我们可以使用 `Symbol.keyFor(sym)`

  例如：

  ```javascript
  let globalSymbol = Symbol.for("name");
  let localSymbol = Symbol("name");
  
  alert( Symbol.keyFor(globalSymbol) ); // name，全局 symbol
  alert( Symbol.keyFor(localSymbol) ); // undefined，非全局
  
  alert( localSymbol.description ); // name
  ```

### 3.8 对象 -- 原始值转换*

- JavaScript 不允许自定义运算符对对象的处理方式。与其他一些编程语言（Ruby，C++）不同，我们无法实现特殊的对象处理方法来处理加法（或其他运算）。

  在此类运算的情况下，对象会被自动转换为原始值，然后对这些原始值进行运算，并得到运算结果（也是一个原始值）。

- 转换规则
  - 没有转换为布尔值。所有的对象在布尔上下文（context）中均为 `true`，就这么简单。只有字符串和数字转换。
  - 数字转换发生在对象相减或应用数学函数时。例如，`Date` 对象可以相减，`date1 - date2` 的结果是两个日期之间的差值。
  - 至于字符串转换 —— 通常发生在我们像 `alert(obj)` 这样输出一个对象和类似的上下文中。

- 对象到原始值的转换，是由许多期望以原始值作为值的内建函数和运算符自动调用的。

  这里有三种类型（hint）：

  - `"string"`（对于 `alert` 和其他需要字符串的操作）
  - `"number"`（对于数学运算）
  - `"default"`（少数运算符，通常对象以和 `"number"` 相同的方式实现 `"default"` 转换）

  规范明确描述了哪个运算符使用哪个 hint。

  转换算法是：

  1. 调用 `obj[Symbol.toPrimitive](hint)` 如果这个方法存在，
  2. 否则，如果 hint 是`"string"`
     - 尝试调用 `obj.toString()` 或 `obj.valueOf()`，无论哪个存在。
  3. 否则，如果 hint 是`"number"`或者`"default"`
     - 尝试调用 `obj.valueOf()` 或 `obj.toString()`，无论哪个存在。

  所有这些方法都必须返回一个原始值才能工作（如果已定义）。

- 在实际使用中，通常只实现 `obj.toString()` 作为字符串转换的“全能”方法就足够了，该方法应该返回对象的“人类可读”表示，用于日志记录或调试。

## 4. 数据类型

### 4.1 原始类型的方法

- 除 `null` 和 `undefined` 以外的原始类型都提供了许多有用的方法。我们后面的章节中学习这些内容。

  - 例如，字符串方法 [str.toUpperCase()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) 返回一个大写化处理的字符串。

    用法演示如下：

    ```javascript
    let str = "Hello";
    
    alert( str.toUpperCase() ); // HELLO
    ```

  - 很简单，对吧？以下是 `str.toUpperCase()` 中实际发生的情况：

    1. 字符串 `str` 是一个原始值。因此，在访问其属性时，会创建一个包含字符串字面值的特殊对象，并且具有可用的方法，例如 `toUpperCase()`。
    2. 该方法运行并返回一个新的字符串（由 `alert` 显示）。
    3. 特殊对象被销毁，只留下原始值 `str`。

- 从形式上讲，这些方法通过临时对象工作，但 JavaScript 引擎可以很好地调整，以在内部对其进行优化，因此调用它们并不需要太高的成本。

- **构造器** `String/Number/Boolean` **仅供内部使用**

  - 像 Java 这样的一些语言允许我们使用 `new Number(1)` 或 `new Boolean(false)` 等语法，明确地为原始类型创建“对象包装器”。

    在 JavaScript 中，由于历史原因，这也是可以的，但极其 **不推荐**。因为这样会出问题。

    例如：

    ```javascript
    alert( typeof 0 ); // "number"
    
    alert( typeof new Number(0) ); // "object"!
    ```

    对象在 `if` 中始终为真，所以此处的 alert 将显示：

    ```javascript
    let zero = new Number(0);
    
    if (zero) { // zero 为 true，因为它是一个对象
      alert( "zero is truthy?!?" );
    }
    ```

  - 另一方面，调用不带 `new`（关键字）的 `String/Number/Boolean` 函数是可以的且有效的。它们将一个值转换为相应的类型：转成字符串、数字或布尔值（原始类型）。

    例如，下面完全是有效的：

    ```javascript
    let num = Number("123"); // 将字符串转成数字
    ```

- **null/undefined 没有任何方法**

### 4.2 数字类型

要写有很多零的数字：

- 将 `"e"` 和 0 的数量附加到数字后。就像：`123e6` 与 `123` 后面接 6 个 0 相同。
- `"e"` 后面的负数将使数字除以 1 后面接着给定数量的零的数字。例如 `123e-6` 表示 `0.000123`（`123` 的百万分之一）。

对于不同的数字系统：

- 可以直接在十六进制（`0x`），八进制（`0o`）和二进制（`0b`）系统中写入数字。
- `parseInt(str，base)` 将字符串 `str` 解析为在给定的 `base` 数字系统中的整数，`2 ≤ base ≤ 36`。
- `num.toString(base)` 将数字转换为在给定的 `base` 数字系统中的字符串。
- **使用两个点来调用一个方法**
  - 请注意 `123456..toString(36)` 中的两个点不是打错了。如果我们想直接在一个数字上调用一个方法，比如上面例子中的 `toString`，那么我们需要在它后面放置两个点 `..`。
  - 如果我们放置一个点：`123456.toString(36)`，那么就会出现一个 error，因为 JavaScript 语法隐含了第一个点之后的部分为小数部分。如果我们再放一个点，那么 JavaScript 就知道小数部分为空，现在使用该方法。
  - 也可以写成 `(123456).toString(36)`。

对于常规数字检测：

- `isNaN(value)` 将其参数转换为数字，然后检测它是否为 `NaN`
- `isFinite(value)` 将其参数转换为数字，如果它是常规数字，则返回 `true`，而不是 `NaN/Infinity/-Infinity`

要将 `12pt` 和 `100px` 之类的值转换为数字：

- 使用 `parseInt/parseFloat` 进行“软”转换，它从字符串中读取数字，然后返回在发生 error 前可以读取到的值。

小数：

- 使用 `Math.floor`，`Math.ceil`，`Math.trunc`，`Math.round` 或 `num.toFixed(precision)` 进行舍入。
- 请确保记住使用小数时会损失精度。

更多数学函数：

- 需要时请查看 [Math](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Math) 对象。这个库很小，但是可以满足基本的需求。

### 4.3 字符串

### 4.4 数组

### 4.5 数组方法

### 4.6 Iterable object（可迭代对象）

### 4.7 Map and Set（映射和集合）

### 4.8 WeakMap and WeakSet（弱映射和弱集合）

### 4.9 Object.keys，values，entries

### 4.10 解构赋值

### 4.11 日期和时间

### 4.12 JSON方法，toJSON
