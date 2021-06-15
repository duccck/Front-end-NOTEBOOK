# 第1章
### 1.2.2 特殊数字

 - infinity
 - -infinity
 - NaN

## 1.4 一元运算符

 - typeof

## 1.5 布尔值
字符串的比较基于 Unicode 标准实现，在比较字符串时，JavaScript 从左往右逐个比较每个字符对应的数字编码

在 JavaScript 中，只有一个值不等于它本身，那就是 NaN
## 1.6 未定义值

 - null
 - undefined

## 1.7 自动类型转换

    console.log(null * 8) //0，null -> 0
    console.log("5" + 1) //6，"5" -> 5
    console.log("five" + 1) //NaN，“five” -> NaN
    console.log(null == undefined) //true
    
    0 / NaN / "" -> false，其他值为 true

 - || 运算，左侧为 true 时，返回左侧的值，否则返回右侧的值
 - && 运算，左侧为 false 时，返回左侧的值，否则返回右侧的值

# 第2章
## 2.2 变量
变量名组成：

 - 字母
 - 数字
 - $
 - _

变量名不能以数字开头
## 2.8 prompt 和 confirm 函数

 - confirm：用户点击“OK”，返回 true；用户点击“Cancel”，返回 false
 - prompt：将用户输入内容作为字符串返回

# 第3章
## 3.1 定义函数
当程序执行到 return 语句时，会立刻跳出当前函数，并将返回值赋值给函数的调用者。如果return 后面没有任何表达式，则返回 undefined
## 3.5 符号声明
函数声明会被提升至其作用域顶端
## 3.7 可选参数

 - 传递多余参数时，多余参数会被忽略
 - 传递参数少于要求数量时，遗漏参数会被赋值为 undefined

## 3.8 闭包

    function wrapValue(x) {
      return function(y) { return x * y; };
    }
    
    var f = wrapValue(5);
    console.log(f(5)); //25
可以把 function 当作一种“冻结”代码，并将其打包成函数值的模型。“return function() {}”可以理解为一个句柄，其指向一段包装好的计算代码

本例中，wrapValue 返回了一段打包好的代码片段，并将其存储在变量 f 中。最后一行调用了存储在该变量中的值，即 wrapValue 创建的代码片段，随后将代码片段解包，并将5作为参数 y 传递给这段代码
