## 1.1 RegExp 的组成
正则表达式包含**模式（pattern）**、可选**修饰符**两部分

    // 可以使用变量作为模式
    // 可以从字符中动态构造模式
    let regexp = new RegExp("pattern", "flags")
    let search = prompt("What do you want to search?", "love");
    let regexp = new RegExp(search);
    "I love JavaScript".search(regexp);
    
    let regexp = /pattern/[flags]

## 1.2 修饰符（flags）

 1. i，搜索时不区分大小写
 2. g，搜索时查找所有匹配项
 3. m，多行模式
 4. u，开启完整的 Unicode 支持
 5. y，粘滞模式

## 1.3 字符类（Character classes）

|符号|匹配项|反向类
|--|--|--|
|.|除`换行符`以外的任何字符；带有 `s` 修饰符时，则可以匹配任何字符|
|\d|任意一个数字|\D|
|\w|拉丁字母、数字、下划线 `_`|\W|
|\s|空格符号：包括空格，制表符 `\t`，换行符 `\n` 和其他少数稀有字符，例如 `\v`，`\f` 和 `\r`|\S|

*注：[\^] 和 [\s\S] 等相反符号组合可匹配任何字符*
## 1.4 Unicode 修饰符 `u`
修饰符 `u` 在正则表达式中提供对 Unicode 的支持

 1. 4 个字节长的字符被以正确的方式处理：被看成单个的字符，而不是 2 个 2 字节长的字符
 2. Unicode 属性可以被用于查找 `\p{…}`

## 1.5 锚点（Anchors）

 - ^，匹配文本开始
 - $，匹配文本结束

*注：修饰符 `m` 会改变锚点的行为*
## 1.6 多行模式 `m`
修饰符 `m` 会改变锚点的行为：在多行模式下，锚点不仅匹配整个文本的开始和结束，还匹配每一行的开始和结束
## 1.7 词边界（Boundary）
有三种不同的位置可作为词边界

- 在字符串开头，第一个字符是字符 `\w`
- 在字符串中的两个字符之间，其中一个是字符 `\w`，另一个不是
- 在字符串末尾，最后一个字符是字符 `\w`

---

    “Hello, Java!".match(/\bJava\b/);
    // |Hello|, |Java|! 竖线位置为 \b 对应位置

*注：只适用于 `\w` 所表示的字符*
## 1.8 转义
特殊字符列表：`[] \ ^ $ . | ? * + ( )`（`/` 在使用 `/.../` 创建正则表达式时需要转义)
在使用 new RegExp() 创建正则表达式实例时，还需要做一些额外的转义工作

    ”5.1".match(new RegExp("\d\.\d")); // null
    ”5.1".match(new RegExp("\\d\\.\\d")); // 5.1

在 JavaScript 中，`\n` `\u1234` 这类需要转义的特殊字符才能消费 `\`，其他不需要转义的字符前的 `\` 会被忽略，`\d` => `d`
## 1.9 集合和范围（Groups and Ranges）

 1. **范围**：`[abc]`，匹配 a b c 中任意一个字符
 2. **集合**：`[0-9a-z]`，匹配 0 到 9 或 a 到 z 中任意一个字符
 3. **排除范围**：`[^\da-z]`，排除数字及 a 到 z 的字符

*注：除了在方括号中有特殊含义的字符外（在开头位置表示排除的 `^`），其它特殊字符不需要转义*
## 1.10 量词（Quantifiers）

 1. {n[,[m]]}
 2. \+（等同于 {1,}）
 3. ?（等同于 {0, 1}）
 4. \*（等同于 {0,}）

## 1.11贪婪量词（Greed）和惰性量词
### 1.11.1 贪婪模式

    let reg = /".+"/g;
    let str = 'a "witch" and her "broom" is one';
    alert(str.match(reg)); // "witch" and her "broom"

#### 回溯（Backtracking）
正则表达式引擎工作原理：

 1. 模式中的第一个 `"` 从字符串的第一个字符开始匹配，直到匹配字符串的第一个 "
 2. 引擎尝试搜索模式中的下一个字符 `.`，对应字符串中的 w
 3. 由于量词 `+`，所以 `.` 不断重复，直至本示例中结尾 e 停止
 4. 引擎完成 `.+` 的搜索，继续搜索模式中的下一个字符 `"`
 5. 由于已经遍历到字符串末尾，没有更多的字符了，引擎知道 `.+` 匹配太多项了
 6. 引擎开始回溯，这一次引擎将 `.+` 匹配减少了一项，在字符串末尾 b 停止，引擎发现还是无法匹配模式中的 `"`
 7. 引擎继续回溯，直至匹配到字符串末尾的 “

*注：回溯对性能的影响*
### 1.11.2 懒惰模式
在量词之后加 `?` 可以启用懒惰模式，正则表达式引擎只会在必要时增加重复次数
**替代方法**
在某些情况下，懒惰模式也不适应，本示例可以使用 `”[^"]+"` 替代懒惰模式

    let str = '...<a href="link1" class="wrong">...<p style="" class="doc">...';
    let reg = /<a href=".*?" class="doc">/g; // 懒惰模式
    
    str.match(reg); // wrong result! <a href="link1" class="wrong">... <p style="" class="doc">

## 1.12 捕获组（Capturing Group）
模式的一部分用 `()` 括起来，这称作捕获组

- 它允许将匹配的一部分作为结果数组中的单独项
- 如果将量词放在括号后面，则它们被视为一个整体

### 1.12.1 搜索所有具有组的匹配项：matchAll
模式中使用 `g` 修饰符时，`match` 只能返回一个结果数组，但没有捕获组的内容（没有 `g` 时，返回捕获组）。此时需要使用 `matchAll`，它与 `match` 的区别：

 1. 返回的不是数组，而是一个可迭代对象
 2. 使用 `g` 修饰符时，将每个匹配组作为一个数组返回
 3. 没有匹配项时，返回一个空的可迭代对象，而不是空数组

可使用 `for-of` `Array from()` `...` `解构赋值` 来使用这个可迭代对象
#### Iterator versus Array

> Many use cases may want an array of matches - however, clearly not all
> will. Particularly large numbers of capturing groups, or large
> strings, might have performance implications to always gather all of
> them into an array. By returning an iterator, it can trivially be
> collected into an array with the spread operator or  `Array.from`  if
> the caller wishes to, but it need not.

    for (const match of 'axxxxxxxxxxxxxxxxxy'.matchAll(/a|(x+x+)+y./g)) {
      if (match[0] === 'a') {
        console.log('Breaking out');
        break;
      }
    }
    console.log('done');

`matchAll` 是懒惰的。正则表达式只会在迭代前一个匹配项后评估字符串的下一个匹配项。这意味着，如果正则表达式开销很大，则可以提取前几个匹配项，然后让迭代器避免尝试进一步匹配。如果是返回的是数组，则需要立刻迭代所有匹配项，无法跳出

> `matchAll` 是一个新方法，可能需要使用 polyfill
> `matchAll` 必须搭配 `g` 一起使用，否则报错？

### 1.12.2 命名组
let regexp = /(`?<name>`xxx)/
命名组为匹配组 group 属性（该属性为一个对象）的属性
### 1.12.3 替换捕获组
`str.replace(regexp, replacement)` 可以用 replacement 来替换 str 中匹配 regexp 的所有捕获组，这使用 `$n` 来完成，n 为捕获组序号（$0 永远为完整匹配项）。对于命名组，使用 `$<name>`
### 1.12.4 非捕获组 ?:
有时需要括号才能正确使用量词，同时不希望其成为捕获组，此时需要在括号开头使用 (`?:`xxx)
## 1.13 模式中的反向引用
不仅可以在结果或替换字符时使用捕获组的内容，还可以在模式自身中使用

    let str = `"She's the one!"`;
    let regexp = /['"](.*?)['"]/g;
    str.match(regexp); // "she'

本例中为了确保模式查找的结束引号和开始引号相同，可以将开始引号包装到捕获组中，并对其反向引用：`/(['"](.+?)\1)/g`。正则表达式引擎会记住匹配到的第一个引号。`\1` 代表第一个捕获组，在模式中表示“查找与第一个捕获组内容相同的文本”
### 1.13.1 按命名反向引用
对于命名组，可以使用 `\k<name>`
## 1.14 选择（OR）
`|` 与 `[xy]`：`|` 在表达式级别生效，而非字符级别
## 1.15 前瞻断言和后瞻断言
### 1.15.1 前瞻（否定）断言
现在需要从 "1 turkey costs 30€" 中获取 ”€“ 前的数字，则需要使用前瞻断言

    x(?=y) // 匹配后面紧跟 y 的 x
    x(?!y) // 前瞻否定断言，匹配后面没有 y 的 x

### 1.15.2 后瞻（否定）断言

    (?<=y)x // 匹配前面是 y 的 x
    (?<!y)x // 匹配前面不是 y 的 x

通常环视断言括号中的内容不会出现在匹配结果中，如果需要，可以将其内容放入括号中（`x(?=(y))`）则会被一同捕获。前瞻断言里括号内容会按顺序出现在捕获组中，后瞻断言里括号内容会最后出现
## 1.16 灾难性回溯

    /^(\d+)*$/.test("012345678901234567890123456789!"); // CPU 消耗 100% 算力，程序被”挂起“

### 1.16.1 使用前瞻断言防止回溯
## 1.17 修饰符 y

    let str = 'let varName = "value"';
    
    let regexp = /\w+/g;
    regexp.lastIndex = 3;
    let result = regexp.exec(str);
    result[0]; // "varName"
    result.index; // 4
    
    let regexp = /\w+/y;
    regexp.last Index = 3;
    let result = regexp.exec(str); // null

使用修饰符 `y` 可以在特定位置进行匹配并停止，而不会同 `g` 一样持续搜索至文本末尾
## 1.18 正则表达式相关方法
### 1.18.1 str.match(regexp)

 - 没有 `g` 标志，返回包含第一个匹配项的数组，包含分组等信息
 - 存在 `g` 标志，返回包含所有匹配项的数组，没有其他信息
 - 没有匹配项时，返回 `null`

### 1.18.2 str.matchAll(regexp)

 - 模式必须使用 `g` 标志，否则报错
 - 返回一个（空）可迭代对象，可使用 `for-of` `Array from()` `...` `解构赋值` 来使用这个可迭代对象
 - 每个匹配项以包含分组等信息的数组返回

### 1.18.3 str.split(regexp|substr, limit)
### 1.18.4 str.search(regexp)

 - 返回第一个匹配项的位置，没有则返回 -1

### 1.18.5 str.replace(regexp|str, str|function)

 - 没有 `g` 标志，返回第一个匹配项被替换后的文本
 - 存在 `g` 标志，返回所有匹配项被替换后的文本
 - 第一个参数是字符串时，只会替换第一个匹配项
 - 第一个参数是正则表达式时，第二个参数可以是特殊字符
 - 第二个参数可以是函数，每次匹配都会调用这个函数，返回值作为替换字符串插入

|符号|替换操作|
|--|--|
|$&|插入整个匹配项|
|$`|在匹配项之前插入字符串的一部分|
|$'|在匹配项之后插入字符串的一部分|
|$n|插入分组 n|
|$\<name>|插入命名分组 name|
|$$|插入字符 $|

    function(match[, n1, n2, ..., offset, input, groups])
    // match，匹配项
    // n1, n2...，分组序号
    // offset，匹配项的位置
    // input，源字符串
    // groups，命名分组对象

### 1.18.6 regexp.exec(str)

- 没有 `g` 标志，返回包含第一个匹配项的数组，包含分组等信息
- 存在 `g` 标志，调用 regexp.exec(str) 会返回第一个匹配项，并将紧随其后的位置保存在属性 regexp.lastIndex 中。下一次调用会从位置 regexp.lastIndex 开始搜索，返回下一个匹配项，并将其后的位置保存在 regexp.lastIndex
- 没有匹配项时，返回 `null`，regexp.lastIndex 被重置为 0
- 可以手动设定 regexp.lastIndex 的值，继而 regexp.exec() 从指定位置开始搜索。搭配 `y` 标志，则可以只搜索指定位置

### 1.18.7 regexp.test(str)
存在 `g` 标志时，regexp.test(str) 会根据 regexp.lastIndex 属性值开始搜索，然后更新 regexp.lastIndex

*注：对于同一个正则表达式，在使用循环或对多个字符串使用时，需要注意 `lastIndex` 的值*
