# 第1章 什么是 JavaScript
## 1.2 JavaScript 实现
完整的 JavaScript 实现包含以下几个部分：

 - ECMAScript（核心）
 - DOM
 - BOM

浏览器只是 ECMAScript 实现可能存在的一种宿主环境，提供 ECMAScript 的基准实现和与环境自身交互必须的扩展。扩展（比如 DOM）使用ECMAScript 核心类型和语法，提供特定于环境的额外功能。其他宿主环境还有 Node.js 和 Adobe Flash

如果不涉及浏览器的话，ECMA-262 描述这门语言的如下部分：

 - 语法
 - 类型
 - 语句
 - 关键字
 - 保留字
 - 操作符
 - 全局对象

ECMAScript 只是对实现这个规范描述的所有方面的一门语言的称呼。JavaScript 实现了 ECMAScript，Adobe ActionScript 也实现了 ECMAScript
# 第3章 语言基础
### 3.1.4 严格模式
严格模式是一种不同的 JavaScript 解析和执行模型，ES3 的一些不规范写法在严格模式下会被处理，对于不安全的活动将抛出错误

    //整个脚本启用严格模式
    "use strict"
    
    //指定函数在严格模式下执行
    function example() {
      "use strict"
    }

### 3.3.1 var 关键字

    //省略 var 操作符之后，str2 将成为全局变量（严格模式下会报错）
    function example() {
      var str1 = "hello world";
      str2 = "hi world";
    }

var 操作符定义的变量会成为包含它的函数的局部变量
使用 var 关键字声明的变量会自动提升至函数作用域顶部
可以反复多次使用 var 声明同一个变量
### 3.3.2 let 声明

    //let 声明的范围是块作用域，let 不允许同一个块作用域中出现冗余声明
    if(true) {
      let str = "hello world";
      let str = "hi world"; //SyntaxError
    }
    console.log(str); //ReferenceError
    
    //var 和 let 声明冗余亦不允许
    var str1;
    let str1; //SyntaxError
    
    let str2;
    var str2; //SyntaxError

let 声明的变量不会在作用域中被提升（虽然在解析时 JavaScript 引擎会注意到 let 声明。在 let 声明之前的执行瞬间被称为”暂时性死区“）
let 声明的变量不会成为 window 对象的属性（var 则会）

    //let 定义的迭代变量作用域仅限于 for 循环中，var 则会渗透至循环体外部
    for(let i = 0; i < 1; i++) {
      console.log(i);
    } //0
    
    console.log(i); //i is not defined
    
    //迭代变量保存的是循环退出时的值，所有 i 都是同一个变量
    for(var i = 0; i < 5; i++) {
      setTimeout(() => console.log(i), 0);
    } //5, 5, 5, 5, 5
    
    //每个迭代循环声明一个新的迭代变量，每个 setTimeout 引用的都是不同的变量实例
    for(let i = 0; i < 5; i++) {
      setTimeout(() => console.log(i), 0);
    } //0, 1, 2, 3, 4

### 3.3.3 const 声明

 - const 声明变量时必须同时初始化
 - const 变量不能被重新赋值

const 行为与 let 基本一致，const 声明的限制只适用于其指向的变量的引用，const 声明的对象内部的属性可以被修改
## 3.4 数据类型
6种简单数据类型（原始类型）：

 1. Undefined
 2. Null，
 3. Boolean
 4. Number
 5. String
 6. Symbol

### 3.4.1 typeof 操作符

    console.log(typeof null); //Object，null 是一个空对象的引用

### 3.4.2 Undefined 类型
Undefined 类型只有一个值，就是特殊值 undefined。声明变量但没有初始化时，就相当于给变量赋值 undefined

    //str1 未声明
    console.log(typeof str1); //undefined
    
    let str2;
    console.log(typeof str2); //undefined

### 3.4.3 Null 类型
Null 类型只有一个值，即特殊值 null。逻辑上来说，null 表示一个空对象指针。在定义将来要保存对象值的变量时，建议使用 null 来初始化

    //undefined 值由 null 值派生而来，因此它们表面上相等
    console.log(undefined == null); //true

### Boolean 类型

 - Boolean() 转型函数

|数据类型|转换为 true|转换为 false|
|--|--|--|
|String|非空字符串|空字符串|
|Number|非零数值|0, NaN|
|Object|任意对象|null|
|Null||null|
|Undefined||undefined|

### Number 类型
在 ECMAScript 中，0 === +0 === -0

任何涉及 NaN 的操作始终返回 NaN。NaN 不等于包括 NaN 在内的任何值

 - isNaN()
 - Number()
 - parseInt()
 - parseFloat()

#### isNaN()

 - isNaN("10") //false, "10" -> 10
 - isNaN(true) //false, true -> 1

#### Number() / + 操作符转换规则

 |数据类型|转换值||
 |--|--|--|
|Boolean|true -> 1|false -> 0|
|Null|0||
|Undefined|NaN||
|String|空字符串 -> 0；Number() 会忽略前、后无意义的零和空格：Number(01) -> 1，Number("  01") -> 1，Number("1.0 ") -> 1|字符串含有非数字字符（除了正、负号和16进制格式） -> NaN|
|Object|先调用 valueOf()，如果返回 NaN，则调用 toString()||

    let a = "123";
    let b = 0;
    console.log(typeof a + b); //string
    console.log(typeof b + a); //number

#### parseInt(str [, option ]) 转换规则

 - 字符串开头的空格和零会被忽略（貌似涉及数值转换都是这样）
 - 第一个字符不是正、负号或数值字符（16进制格式除外） -> NaN
 - 空字符串 -> NaN
 - 检测到非数值字符停止
 - option 为进制数（2，8，10，16），建议始终传入第二个参数 parseInt(str, 10)

#### parseFloat() 转换规则

 - 第一次出现的小数点有效，其余无效
 - 字符串开头的空格和零会被忽略
 - 只解析十进制，十六进制始终返回 0
 - parseFloat(" 01.0 1") -> 1，小数点后面的零无意义时会被忽略

### 3.4.6 String 类型
#### 字符字面量

|字面量|含义|
|--|--|
|\n|换行|
|\t|制表|
|\r|回车|
|\xnn|以十六进制编码 nn 表示的字符，如 \x41 等于 “A”|
|\unnnn|以十六进制编码 nnnn 表示的 Unicode 字符，如 \u0040 等于 “A"|
|\\\\|反斜杠|
|\\`|单引号|
|\\"|双引号|
|\\'|反引号|
|\b|退格|
|\f|换页|

这些字符字面量在字符串中作为单个字符被解释，字符长度为1
#### 字符串是不可变的（immutable）
在 ECMAScript 中，字符串是不可变的，一旦创建，它们的值就不能变了。要修改某个字符串值，必须先销毁原始字符串，然后将包含新值的另一个字符串保存到该变量

    let lang = "Java";
    lang = lang + "Script";
    //分配一个能够容纳10个字符的空间，填充上“JavaScript”，然后销毁原始字符串“Java”和“Script”

#### 转换为字符串

 - toString([option]) 方法
   - option 仅适用与数值，用于接收一个底数（任意进制） 
   - Number / Boolean / Object / String 均具备该方法
   - Null / Undefined 没有该方法
 - String() 函数
   - 在不确定值是否为 null 或 undefined 时，可使用 String() 函数
   - 当值有 toString() 方法时，调用该方法（不传参）
   - 当值为 null，返回"null"
   - 当值为 undefined，返回"undefined"
 - \+ 操作符
   - 给一个值加上一个空字符串

#### 模板字面量
从技术上来说，模板字面量不是字符串，而是一种特殊的 JavaScript 句法表达式，只不过在求值后得到的是字符串。
#### 字符串插值 ${ expression }
模板字面量在定义时立刻求值并转换为字符串实例，任何插入的变量也会从它们最接近的作用域中取值

 - 所有插入值会使用 toString() 强制转换为字符串
 - 嵌套模板字符串无须转义
   - \`Hello, ${\`world\`} \`
 - 插值表达式中可以调用函数或方法

#### 标签函数

    let name = "Allen";
    let age = 27;
    
    //function tag(argu1, ...arguments)
    //使用剩余操作符将表达式参数收集到一个数组中
    function tag(argu1, argu2, argu3) {
      console.log(argu1);
      console.log(argu2);
      console.log(argu3);
    }
    
    tag`I am ${name}, my age is ${age}`;
    //等价于 tag(["I am",", my age is", ""], name, age)
    //["I am",", my age is", ""]
    //Allen
    //27

标签函数的第一个参数是被嵌入表达式分隔的文本的数组，后面的参数是嵌入表达式
#### 原始字符串
使用模板字面量可以直接获取原始的模板字面量内容，而不是被转换后的字符表示

 - String.raw 标签函数

    console.log(\`first line\nsecond line\`);
    //first line
    //second line
    
    console.log(String.raw\`first line\nsecond line\`);
    //first line\nsecond line

### 3.4.7 Symbol 类型
符号实例是唯一、不可变的。符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突
### 3.4.8 Object 类型
对象是一组数据和功能的集合，Object 是派生其他对象的基类

    let o = new Object();

#### Object instances' properties and methods

 - constructor
 - hasOwnProperty("propertyName") //判断当前对象实例是否存在给定的属性
 - isPrototypeOf(object) //判断当前对象是否为给定对象的原型
 - propertyIsEnumerable("propertyName") //判断属性是否可以使用 for-in 枚举
 - toLocaleString() //返回对象的字符串表示，该字符串反映对象所在的本地化执行环境（如 Date / Number 对象在不同国家或地区的本地化表示）
 - toString() //返回对象的字符串表示
 - valueOf() //返回对象的字符串、数值或布尔值表示，通常与 toString() 相同

ECMA-262 中对象的行为不一定适合 JavaScript 的其他对象。如浏览器环境中的 BOM 和 DOM 对象，都是由宿主环境定义和提供的宿主对象。宿主对象不受 ECMA-262 约束，所以它们可能不会继承 Object
## 3.5 操作符
### 3.5.1 一元操作符
#### 递增 / 递减操作符

 1. 前缀版，变量的值在语句被求值前改变
 2. 后缀版，变量的值在语句被求值后改变

该操作符可用于任何值（Number / String / Boolean / Object）

|类型|转换值|
|--|--|
|String|有效数值形式：转换为数值再做改变；不是有效数值形式：变量值设为 NaN；变量变为 Number 类型|
|Boolean|false：转换为0再做改变；true：转换为1再做改变；变量 -> Number|
|Object|调用 valueOf() 取得可以操作的值，再对值应用上述规则；如果是 NaN，则调用 toString() 并再次应用上述规则；变量 -> Number|

#### 一元加、减操作符
将一元加或一元减应用到非数值时，类型转换规则与 Number() 相同，其中一元减会使结果为负值
### 3.5.2 位操作符
位操作符用于数值的底层操作，也就是操作内存中表示数据的 bit（位），因此速度快得多

ECMAScript 中的所有数值都以 IEEE 754 64位格式存储，但位操作并不直接应用到64位，而是先把值转换为32位整数，再进行位操作，之后再把结果转换为64位

有符号整数前31位表示整数值，第32位为符号位。正值以真正的二进制格式存储，即31位中的每一位都代表2的幂。负值以一种称为补码的二进制编码存储（反码加1）

 1. 确定绝对值的二进制表示
 2. 按位取反
 3. 结果加1

十进制转换为二进制不足32位时，如果是正数，左侧以0填充；负数时，以1填充
NaN / Infinity 在位操作中会被当成0处理

如果将位操作应用到数值，首先会使用 Number() 将该值转换为数值，然后进行位操作，最终结果为数值
#### 按位非（~）

    let num1 = 25;
    let num2 = ~num1;
    //等同于对数值取反并减1
    //let num2 = -num1 - 1;
    console.log(num2); //-26

#### 按位与（&）
都为真才为真
#### 按位或（|）
有一个为真即为真
#### 按位异或（^）
相同（都为1）为0，不同（一个1，一个0）为1
#### 左移（<<）
将数值的所有位向左移，数值右端空出的位置以0填充
#### 有符号右移（>>）
将数值的所有32位向右移，同时保留符号位，左侧空出来的位置以符号位填充
#### 无符号位右移（>>>）
将数值的所有32位向右移，空位补0，而不管符号位是什么
### 3.5.3 布尔操作符
#### 逻辑非（!）
不论应用到什么类型，均返回布尔值。逻辑非操作符首先将操作数转换为布尔值，然后对其取反

|数据类型|结果|
|--|--|
|Object|false|
|空字符串|true|
|非空字符串|false|
|0|true|
|非0数值（包括 Infinity）|false|
|null|true|
|NaN|true|
|undefined|true|

同时使用 !! 可将任意值转换为布尔值，等同于 Boolean()
#### 逻辑与（&&）
**短路特性**
当第一个操作数为 false，第二个操作数永远不会求值

如果有操作数不是布尔值，则逻辑与不一定会返回布尔值

|数据类型|结果|
|--|--|
|第一个操作数是对象|返回第二个操作数|
|第二个操作数是对象|第一个操作数为 true 时，才返回该对象|
|两个操作数都为对象|返回第二个操作数|
|第一个操作数为 null / NaN / undefined，或者第一个操作数为 true，第二个操作数为 null / NaN / undefined|返回 null / NaN / undefined|

#### 逻辑或（||）
第一个操作数为 false / null / NaN / undefined 时，返回第二个操作数
### 3.5.4 乘性操作符
在处理非数值时，会使用 Number() 将该操作数转换为数值
#### 乘法操作符（*）

|数据类型|结果|
|--|--|
|任一操作数为 NaN|返回 NaN|
|Infinity * 0|返回 NaN|
|Infinity * 非0有限数值|(-)Infinity|
|Infinity * Infinity|返回 Infinity|

#### 除法操作符（/）

|数据类型|结果|
|--|--|
|任一操作数为 NaN|返回 NaN|
|0 / 0|返回 NaN|
|非0有限数值 / 0|(-)Infinity|
|Infinity / 任何数值|(-)Infinity|
|Infinity / Infinity|返回 NaN|

#### 取模操作符（%）

|数据类型|结果|
|--|--|
|无限值 % 有限值|返回 NaN|
|有限值 % 0|返回 NaN|
|有限值 % 无限值|返回该有限值|
|0 % 非0|返回0|
|Infinity % Infinity|返回 NaN|

#### 指数操作符（**）

    let number = 2;
    number **= 2;
    
    //等同于
    number = Math.pow(number, 2);

### 3.5.6 加性操作符
#### 加法操作符

|数据类型|结果|
|--|--|
|任一操作数为 NaN|返回 NaN|
|+0 + +0|+0|
|+0 + -0|+0|
|-0 + -0|-0|
|(-)Infinity + (-)Infinity|(-)Infinity|
|Infinity + -Infinity|返回 NaN|
|只有一个操作数是字符串|将另一个操作数转换为字符串，再拼接|
|任一操作数是 object / boolean / number|调用 toString()|
|undefined / null|调用 String() 得 “undefined / null”|

#### 减法操作符

|数据类型|结果|
|--|--|
|任一操作数为 NaN|返回 NaN|
|+0 - +0|+0|
|+0 - -0|-0|
|-0 - -0|+0|
|(-)Infinity - (-)Infinity|返回 NaN|
|Infinity - -Infinity|Infinity|
|-Infinity - Infinity|-Infinity|
|任一操作数是 string / boolean / undefined / null|调用 Number() 将其转换为数值，再作数学计算|
|任一操作数是 object|调用 valueOf() 取得数值，如果为 NaN，则计算结果为 NaN；如果没有 valueOf() 方法，则调用 toString()，再将得到的字符串转换为数值|

### 3.5.7 关系操作符

|数据类型|结果|
|--|--|
|任一操作数为 NaN|返回 false|
|操作数都为 String|逐个比较字符所对应的编码大小|
|任一操作数为 Number|将另一操作数转换为数值|
|任一操作数为 Object|调用 valueOf()，没有则调用 toString()，取得结果后按上述规则比较|
|任一操作数为 Boolean|将其转换为数值|

### 3.5.8 相等操作符

|类型|是否类型转换|
|--|--|
|== / !=|是|
|=== / !==|否|

#### == / != 转换规则

|数据类型|结果|
|--|--|
|任一操作数为 NaN|== 为 false；!= 为 true|
|任一操作数为 Boolean|将其转换为数值|
|一个操作数为 String，另一个为 Number|将字符串转换为数值|
|只有一个操作数是 Object|调用 valueOf() 取得值，再应用上述规则|
|操作数均为 Object|为同一个对象返回 true；否则返回 false|
|null == undefined||
|null / undefined 不能转换为其他值||

### 3.5.11 逗号操作符
逗号操作符用来在一条语句中执行多个操作

    //辅助赋值，在赋值时使用逗号分隔，最终会返回最后一个值
    let num = (5, 1, 4, 0); //num 值为0

## 3.6 语句
### 3.6.5 for-in 语句
for-in 语句是一种严格的迭代语句，用于枚举对象中的非符号键属性（非属性值）

    for(property in expression) statement

ECMAScript 中对象的属性是无序的，因此迭代结果不能保证返回属性的顺序
### 3.6.6 for-of 语句
for-in 语句是一种严格的迭代语句，用于遍历可迭代对象的元素

    for(const el of [1, 2, 3]) {
      console.log(el);
    }
    
    //1
    //2
    //3

### 3.6.7 标签语句
标签语句用于给语句添加标签。一般用于嵌套循环，可通过 break / continue 语句引用，返回代码中特定的位置

    let num = 0;
    
    outermost:
    for(let i = 0; i < 10; i++) {
      for(let j = 0; j < 10; j++) {
        if(i == 5 && j == 5) {
          break outermost;
        }
        num++;
      }
    }

### 3.6.9 with 语句
with 语句用于将代码作用域设置为特定的对象
*由于 with 语句影响性能且难于调试，通常不推荐使用*

    let name = person.name;
    let age = person.age;
    
    //等同于
    with(person) {
      let myName = name;
      let myAge = age;
    }
    
    //myName 和 myAge 属于函数上下文的一部分，而非 person 对象

在上面的语句内部，每个变量首先是一个局部变量。如果没有找到该局部变量，则会搜索 person 对象
### 3.6.10 switch 语句

    switch(expression) {
      case value1:
        statement
        break;
      case value2:
        statement
        break;
      ...
      default:
        statement
    }

ECMAScript 中 switch 可以用于所有数据类型（很多语言，只能用于数值），条件的值可以是变量或表达式
## 3.7 函数
#### return 语句

 - 只要碰到 return 语句，函数会立刻停止执行并退出
 - return 语句可以不带任何返回值，通常用于终止函数执行，而不是为了返回值
 - 函数没有 return 语句或者 return 语句后面不带返回值，默认返回 undefined

#### 严格模式对函数的限制

 - 函数名不能为 eval 或 arguments
 - 函数参数不能叫 eval 或 arguments
 - 两个参数名称不能相同

# 第4章 变量、作用域与内存
## 4.1 原始值与引用值
ECMAScript 变量可以包含两种不同类型的数据：

 - 原始值，6种简单数据类型
 - 引用值，多个值构成的对象

将一个值赋给变量时，JavaScript 引擎必须确定这个值是原始值还是引用值

 - 保存原始值的变量是按值访问的，操作的是存储在变量中的实际值
 - 保存引用值的变量是按引用访问的。引用值是保存在内存中的对象，JavaScript 不允许直接访问内存位置，在操作对象时，实际操作的是对该对象的引用而非对象本身

### 4.1.2 复制值

|类型|操作|
|--|--|
|原始值|将原始值（实际值）复制给另一个变量，两个变量完全独立|
|引用值|将引用值（指向堆内存中的对象的指针）复制给另一个变量，两个变量指向同一个对象|

### 4.1.3 传递参数
ECMAScript 中所有函数的参数都是按值传递的。在按值传参时，值会被复制到一个局部变量（arguments 对象中的一个槽位）

 - 实参是原始值：将实际值复制给形参
 - 实参是对象：将地址指针复制给形参

### 4.1.4 确定类型（instanceof）

|typeof 用于|结果|
|--|--|
|string / number / boolean / undefined|能够返回对应的原始类型|
|null / object|返回 object|

我们通常不关心一个值是否为对象，而是什么类型的对象

    result = variable instanceof constructor

## 4.2 执行上下文与作用域
变量或函数的**上下文**（自己理解：在同一个作用域内的变量或函数组成一个上下文）决定了它们可以访问哪些数据。每个上下文都有一个关联的**变量对象**，这个上下文定义的所有变量和函数都存在于这个对象上

全局上下文是最外层的上下文。在浏览器中，window 对象是全局上下文，通过 var 定义的全局变量和函数会成为 window 对象的属性和方法。使用 let / const 的顶级声明不会定义在全局上下文中。上下文在其代码执行完毕后会被销毁，全局上下文在应用程序退出时才会被销毁

每个函数调用都有自己的上下文，当代码执行流进入函数时，函数上下文被推到一个上下文栈上。在函数执行完之后，上下文栈会弹出该函数上下文，将控制权返还给之前的执行上下文。ECMAScript 程序的执行流就是通过这个上下文栈进行控制

上下文中的代码在执行时会创建变量对象的一个——**作用域链**。作用域链决定了各级上下文中的代码在访问变量和函数时的顺序。代码正在执行的上下文的变量对象始终位于作用域链最顶端。作用域链中的下一个变量对象来自包含上下文，以此类推直至全局上下文，全局上下文的变量对象始终是作用域链中的最后一个变量对象

代码执行时的标识符解析是通过沿作用域链逐级搜索标识符名称完成的，搜索过程始终从作用域链最前端开始
### 4.2.1 作用域链增强
某些语句会导致在作用域链前端临时添加一个上下文：

 - try / catch 语句中的 catch 块，catch 语句会创建一个新的变量对象，这个变量对象会包含要抛出的错误对象的声明
 - with 语句，with 语句会将指定的对象添加到作用域链前端

#### 词法作用域（补充）

    var str = "I'm global scope.";
    
    function f1() {
      console.log(str);
    }
    
    function f2() {
      var str = "I'm local scope."
      
      f1();
    }
    
    f2(); //I'm global scope.

ECMAScript 采用词法作用域（即静态作用域，对应动态作用域），函数的作用域在函数定义时就已经决定了
## 4.3 垃圾回收
### 4.3.1 标记清理

 - 当变量进入上下文，比如在函数内部声明一个变量时，这个变量会被加上存在于上下文的标记
 - 当变量离开上下文时，会被加上离开上下文的标记，垃圾回收期间删除

### 4.3.4 内存管理
#### 内存泄漏
全局变量、定时器、闭包很容易导致内存泄漏
