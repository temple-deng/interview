# JS

## JS 权威指南

### 2 词法结构

1. JS 程序是用 Unicode 字符集编写的，但是编码方式还是 UTF-16
2. JS 会忽略程序中标识 token 之间的空格
3. \u0020 空格符，\u0009 水平制表符，\u000A 换行符，\u000D 回车符
4. 在有些计算机硬件和软件里，无法显示或输入 Unicode 字符全集。为了支持那些使用老旧技术的程序
员，JS 定义了一种特殊序列，使用 6 个 ASCII 字符来代表任意 16 位 Unicode 内码（这里说的很
清楚了，16 位的编码）。这些 Unicode 转义序列均以 \u 为前缀，气候跟随 4 个十六进制数。这种
Unicode 转义写法可以用在 JS 字符串直接量、正则表达式直接量和标识符中（关键字除外）     

```js
var b\u0061 = '123';
ba    // "123"
```    

5. 标识符就是一个名字，用来对变量、函数和循环跳转的 label 语句命名。以字母、$ 和下划线开头，
后面可以跟字母、数字、$ 和下划线。  
6. 在 JS 中，如果语句各自独占一行，通常可以省略语句之间的分号。需要注意的是，JS 并不是在所有
换行处都填补分号：只有在缺少了分号就无法正确解析代码，JS 才会填补分号。也就是说，如果现在这一行
的结尾加上下一行的开头连起来，还可以正常的做语法解析的话，那就不需要当成两条语句。用书上的话说就是，
如果当前语句和随后的非空格字符不能当成一个整体来解析的话，JS 就在当前语句行结束处填补分号
7. 如果当前语句和下一行语句无法合并解析，JS 则在第一行后补充符号，这是通用规则，但有两个例外。
第一个例外是 return, continue 和 break 之后紧跟的换行会立即补充分号
8. 另一个是 ++, -- 如果是后缀表达式，必须在同一行。    

```js
x
++
y     // 解析成 x; ++y
```    

**字符集、分号、标识符、关键字**     

### 3 类型、值和变量

1. 原始类型：数字、字符串、布尔值、null 和 undefined
2. 函数也是对象
3. 只有 null 和 undefined 是无法拥有方法的值
4. JS 中的所有数字均用浮点数值表示，按照这个数字格式，能够表示的整数范围是 -2<sup>53</sup>
~ 2<sup>53</sup>    

![ieee-754](https://raw.githubusercontent.com/temple-deng/markdown-images/master/computer-system/type-of-floating-expression.png)   

5. 被零整除在 JS 中并不报错：它只是简单的返回无穷大或负无穷大，但有一个例外，0除以0返回 NaN
6. 虽然都是用浮点数表示，但是和 C 中一样，整数都是精确地表示，而浮点数有很多近似的表示。   

```js
2 - 1  === 3 - 2;   // true
1.3 - 1.2 === 1.4 - 1.3;   // false
1.0 - 0.5 === 2.0 - 1.5;   // true
1.1 - 0.6 === 2.1 - 1.6    // false
1.3 - 1.2;  // 0.10000000000000009
```    

倒数第三个看起来是因为所有四个数都能用精确表示，因此虽然出现了浮点数值但还是可以精确表示。   

7. 字符串的长度是其所含 16 位值的个数
8. 字符串直接量可以拆分多行，\。
9. null, undefined, 0, -0, '', NaN 会转换为 false
10. null 是关键字，undefined 是预定义的全局变量，也就是全局对象的属性
11. 类型转换    


值 | 转换为字符串 | 数字 | 布尔值 | 对象
---------|----------|---------|---------|---------
 undefined | "undefined" | **NaN** | false | throws TypeError
 null | "null" | **0** | false | throws TypeError
 true | "true" | 1 | - | new Boolean(true)
 false | "false" | 0 | - | new Boolean(false)
 "" | - | 0 | false | new String("")
 "1.2" | - | 1.2 | true | new String("1.2")
 "one" | - | NaN | true | new String("one")
 0 | "0" | - | false | new Number(0)
 -0 | "0" | - | false | new Number(0)
 NaN | "NaN" | - | false | new Number(NaN)
 Infinity | "Infinity" | - | true | new Number(Infinity)
 -Infinity | "-Infinity" | - | true | new Number(-Infinity)
 1 | "1" | - | true | new Number(1)      

12. Number 类定义的 `toString()` 方法可以接收表示转换基数的可选参数。范围是 2~36。
13. `toFixed()` 根据小数点后的指定位数将数字转换为字符串，`toExponential()` 使用指数
计数法将数字转换为指数形式的字符串，其中小数点前一位，小数点后的位数由参数指定，`toPrecision()`
根据指定的有效数字位数将数字转换为字符串。如果有效数字的位数少于数字整数部分的位数，则转换成指数
形式。
14. `parseInt()` 默认是可以解析十六进制数字的，不过也可以传入第二个参数，指定要解析的字符串的进制。
15. `valueOf()` 如果存在任意原始值，它就默认将对象转换为表示它的原始值，对象是复合值，而且大多数
对象无法真正表示为一个原始值，因此默认的 `valueOf()` 方法简单地返回对象本身，而不是返回一个
原始值。
16. 对象到字符串的转换经过了如下的这些步骤：
  + 如果对象具有 `toString()` 方法，则调用这个方法。如果它返回一个原始值，将这个值转换为字符串
  （如果本身不是字符串），并返回这个字符串结果。
  + 如果没有 `toString()` 方法，或者这个方法并不返回一个原始值，那么调用 `valueOf()` 方法。
  如果 `valueOf()` 方法返回原始值，将这个值转换为字符串（本身不是字符串），并返回这个字符串结果。
  + 否则，抛出类型异常错误
17. 对象到数字数字的转换：
  + 如果对象有 `valueOf()` 方法，并且这个方法返回一个原始值，将这个原始值转换为数字（如果必要
  的话）并返回
  + 如果没有 `valueOf()` 方法，有 `toString()` 方法，就把这个方法返回的原始值转换为数字返回
  + 否则，抛出类型异常
18. 上述情况只是通用情况，还有一些特殊的情况，+, ==, != 和关系运算符的执行对象到原始值的转换
时，满足这样的要求：无论是调用的 `valueOf()` 还是 `toString()` 的方法，如果它们返回了原始值，
就直接使用这个原始值，而不是进一步转换为数字或字符串。个人觉得是因为这几种情况即可以针对字符串
运算，又可以针对数字运算，所以无法判定到底哪一个满足需求，如果结果原始值类型不匹配的话，还可以
继续进行隐式的原始值到原始值的转换。
17. JS 全局变量是全局对象的属性，这个 ECMA 规范中强制规定的。
18. 在 JS 的最顶层代码中，作用域链是由一个全局对象组成。在不包含嵌套的函数体内，作用域链上有两个
对象，第一个是定义函数参数和局部变量的对象，第二个是全局对象。在一个嵌套的函数体内，作用域链上
至少有三个对象。当 **定义** 一个函数时，它实际上保存一个作用域链。当调用这个函数时，它 **创建**
一个新的对象来存储它的局部变量，并将这个对象添加至保存的那个作用域链上，同时创建一个新的更长的
表示函数调用的作用域"链".

### 4 表达式和运算符

1. 将简单表达式组合成复杂表达式的方法不只有使用运算符，还有一些不涉及运算符的组合方式，例如访问
数组元素和函数调用。
2. 原始表达式，包括常量、直接量、关键字和变量。
3. 复杂表达式，包括对象和数组的初始化表达式、函数定义表达式、属性访问表达式、调用表达式、对象
创建表达式等。   

![js-operators](https://raw.githubusercontent.com/temple-deng/markdown-images/master/uncategorized/js-operators.png)   

4. 属性访问表达式和调用表达式比表中所有的运算符优先级都要高。
5. 求余运算符的操作数通常是整数，但也适用于浮点数，6.5 % 2.1 = 0.2
6. 位运算符首先将操作数转换为数字，并将数字强制表示为 32 位整型，这会忽略原格式中的小数部分和
任何超过 32 位的二进制位。移位运算符要求右操作数在 0~31 之间。位运算符会将 NaN, Infinity 和
-Infinity 都转换为 0.
7. 关系表达式的运算结果总是为 true 或 false，关系运算符，包括比较运算符、相等和不等运算符、in
和 instanceof
8. null === null, undefined === undefined 都是 true
9. 除非运算场合明确要求需要字符串，否则对象在进行向原始值的转换时，都是先尝试调用 `valueOf()`
方法，之后才是 `toString()`。
10. 除 + 外，其他的运算符都是偏爱数字类型的。
11. `eval()` 只有 1 个参数。如果传入的参数不是字符串，它直接返回这个参数。如果参数是字符串，它
会把字符串当成代码进行编译，如果编译失败则抛出语法错误异常。如果编译成功，就开始执行，并返回字符串
中的最后一个表达式或语句的值，如果最后一个表达式或语句没有值，返回 undefined。如果字符串抛出
一个异常，这个异常会把该调用传递给 `eval()`。
12. 当直接使用非限定的 eval 名称来调用 `eval()` 函数时，通常称为”直接的eval“，它总是在调用它
的上下文作用域内执行。其他的间接调用则使用全局对象作为其上下文作用域，并且无法读、写、定义局部
变量和函数。    

### 5 语句

1. do/while，注意 while 后的分号    

```js
do
  statement
while (expression);
```   

2. 在执行 for/in 语句的过程中，JS 解释器首先计算 object 表达式。如果表达式为 null 或者
undefined，JS 会跳过循环并执行后续的代码，也有部分实现可能会抛出异常。如果表达式等于一个原始值，
这个原始值将会转换为与之对应的包装对象。
3. 标签语句 `identifier: statement`。标签的命名空间和变量或函数的命名空间是不同的，因此可以
使用同一个标识符作为语句标签和作为变量名或函数名。一个语句标签不能和它内部的语句标签重名，但在
两个代码段不相互嵌套的情况下是可以出现同名的语句标签的。
4. 当 break 和标签一块使用时，程序将跳转到这个标签所标识的语句块的结束，或者直接终止这个闭合
语句块的执行。
5. 当由于 return, continue 或 break 语句使得解释器跳出 try 语句块时，解释器在执行新的目标
代码之前先执行 finally 块中的逻辑。
6. 严格模式：
  - 禁止使用 with 语句
  - 变量要先声明，不能直接赋值
  - 调用非方法的函数中的 this 指向 undefined
  - 当通过 `call()` 或 `apply()` 来调用函数时，其中的 this 值就是通过 `call()` 或
  `apply()` 传入的第一个参数
  - 给只读属性赋值和给不可扩展的对象创建新成员都将抛出一个类型错误异常
  - 传入 `eval()` 的代码不能在调用程序的上下文中声明变量或定义函数
  - 函数里的 arguments 对象拥有传入函数值的静态副本
  - delete 后面跟非法的标识符（变量、函数、函数参数）时，将抛出语法错误异常
  - 试图删除一个不可配置属性将抛出异常
  - 在一个对象直接量中定义两个或多个同名的参数将产生一个语法错误
  - 不允许八进制
  - arguments.caller 和 arguments.callee 会抛出异常    

### 6 对象

1. 属性名可以是空字符串
2. 创建对象的三种方法：直接量、构造函数和 `Object.create()` 方法
3. 属性赋值失败的场景：
  + 属性 p 是只读的（无论是继承的还是自有的）
  + o 中不存在自有属性 p：o 没有使用 setter 方法继承属性 p，并且 o 的可扩展性是 false。
4. delete 只能删除自有属性
5. delete 后面的非正常的操作数有两种情况，一种是一个属性访问表达式（但属性不可配置），一种
不是属性访问表达式，在严格模式中，这两种情况都会抛出异常，但在非严格模式中，第一种情况返回 false,
第二种情况返回 true
6. 检测属性的方法，in、`hasOwnProperty()` 和 `propertyIsEnumerable()`
7. 枚举方法，for/in、`Object.keys()` 和 `Object.getOwnPropertyNames()`。
8. 存取器属性：   

```js
var o = {
  get propName() { /*...*/ },
  set propName(value) { /*...*/ }
};
```    

9. 属性描述符：value, writable, configurable, enumerbale, set, get。
10. `Object.getOwnPropertyDescriptor()`, `Object.defineProperty()`,
`Object.defineProperties()`
11. `defineProperty()` 和 `defineProperies()` 违反规则的使用场景：
  + 对象不可扩展，则可以编辑已有属性，但不可新增属性
  + 属性不可配置，则不可修改可配置性和枚举性
  + 如果存取器属性是不可配置的，则不能修改其 getter 和 setter 方法，也不能将它转换为数据
  属性
  + 如果数据属性是不可配置的，则不能将其转换为存取器属性
  + 如果数据属性是不可配置的，则不能将其可写性从 false 设置为 true，但反之可以
  + 如果数据属性是不可配置且不可修改的，则不能修改它的值，然而可配置但不可写属性的值是可以修改的
12. `Object.getPrototypeOf()`, `Object.prototype.isPrototypeOf()`
13. 对象的类属性是一个字符串，默认的 `toString()` 返回了这种格式的字符串：`[object class]`
14. `Object.isExtensiable()`, `Object.preventExtensions()`。一旦将对象转换为不可扩展，
就无法转换回来了，`preventExtensions()` 只影响到对象本身的可扩展性，如果给原型链在属性，还是
能加进去
15. `Object.seal()`, `Object.isSealed()`
16. `Object.freeze()`, `Object.isFrozen()`     

### 7 数组

1. a[1.000] 等价于 a[1]
2. `join(), reverse(), sort(), concat(), slice(), splice(), push(), pop()`,
`unshift(), shift()`
3. `splice()` 返回删除元素组成的数组，`push()` 返回数组新的长度，`pop()` 返回删除的元素，
`unshift()` 和 `shift()` 同理
4. `forEach()` 的提前终止，可以将方法放到一个 try/catch 块中，在提前终止时，抛出一个异常。
5. `map(), filter(), every(), some(), reduce(), reduceRight(), indexOf()`
`lastIndexOf()`
6. 当不指定初始值调用 `reduce()` 时，它使用数组的第一个元素作为初始值，同理，`reduceRight()`
使用最后一个元素。
7. 在空数组上，不带初始值参数调用 `reduce()` 将导致类型错误异常。如果调用它的时候只有一个值——
数组只有一个元素且没有指定初始值，或者有一个空数组并且指定一个初始值，`reduce()` 只是简单地
返回那个值而不会调用化简函数。   

### 8 函数

1. 一条函数声明语句实际上声明了一个变量，并把一个函数对象赋值给它。相对而言，定义函数表达式时
并没有声明一个变量，但是函数定义表达式的值就是新定义的函数，相当于把这个值赋值给了等号左边的变量。    
2. 如果构造函数没有形参，JS 构造函数调用的语法是允许省略实参列表和圆括号的。
3. 在非严格模式中，函数里的 arguments 仅仅是一个标识符，在严格模式中，它变成了一个保留字。
严格模式中的函数无法使用 arguments 作为形参名或局部变量名，也不能给 arguments 赋值。
4. 实参对象定义了 callee 和 caller 属性。callee 指代当前正在执行的函数，caller 指代调用
当前正在执行函数的函数。
5. 词法作用域，也就是说，函数的执行依赖于变量的作用域，这个作用域是在函数定义时决定的，而不是函数
调用时决定的。为了实现这种词法作用域，JS 函数对象的内部状态不仅包含函数的代码逻辑，还必须引用
当前的作用域链。
6. 函数对象通过作用域链相互关联起来，函数体内部的变量都可以保存在函数作用域内，这种特性称为“闭包”
7. 关联到闭包的作用域链都是 **活动的**，记住这一点非常重要。嵌套的函数不会将作用域内的私有
成员复制一份，也不会对所绑定的变量生成静态快照。这种活动的特性，可能在一些情况下导致定义时的
作用域链内保存的数值与调用时的不一致，导致一些奇怪的现象。
8. 传入 `apply()` 的参数数组可以是类数组对象也可以是真实数组。
9. bind:   

```js
if (!Function.prototype.bind) {
  Function.prototype.bind = function(o) {
    var self = this;
    var args = [].slice.call(arguments, 1);
    
    return function() {
      var newArgs = args.concat([].slice.call(arguments));
      return this.apply(o, newArgs);
    }
  }
}
```    

10. `Function()` 构造函数可以传入任意数量的字符串实参，最后一个实参所表示的文本就是函数体；它
可以包含任意的 JS 语句，没两条语句之间用分号分隔。
11. `Function()` 构造函数创建的函数并不是使用词法作用域，相反，函数体代码的编译总是在全局
作用域执行。    

### 9 类和模块

1. 调用构造函数的一个重要特征是，构造函数的 prototype 属性被用做新对象的原型。这意味着通过
同一个构造函数创建的所有对象都继承自一个相同的对象，因此它们是同一个类的成员。
2. 原型对象是类的唯一标识：当且仅当两个对象继承自同一个原型对象时，它们才是属于同一个类的实例。
而初始化对象的状态的构造函数则不能作为类的标识，两个构造还是你的 prototype 属性可能指向同一个
原型对象。那么这两个构造函数创建的实例是属于同一个类的。
3. 每个 JS 函数（`bind()` 返回的函数除外）都自动拥有一个 prototype 属性。这个属性的值是一个
对象，这个对象包含唯一一个不可枚举属性 constructor。

### 10 正则表达式

1. 正则表达式中的所有字母和数字都是按照字面含义进行匹配的。也支持非字母的字符匹配，这些字符需要
通过反斜线作为前缀进行转义。
2. 具有特殊含义的符号：`^ $ . * + ? = ! : | \ / ( ) [ ] { }`，某些符号只有在某些正则表达式
的上下文中才具有某种特殊含义，其他上下文中则被当成直接量进行处理。
3. 将直接量字符单独放进方括号内就组成了字符类。一个字符类可以匹配它所包含的任意字符。否定字符类。
4. 由于某些字符类非常常用，因此在正则语法中，使用了这些特殊字符类的转义字符来表示它们：
  - . 除换行符和其他 Unicode 行终止符之外的任意字符
  - \w，任何 ASCII 字符组成的单词，等价于 [a-z0-9A-Z]，\W
  - \s 任何 Unicode 空白符, \S
  - \d 数字，\D
  - [\b] 退格直接量
5. 在方括号之内也可以写这些特殊转义字符
6. 我们在正则模式之后跟随用以指定字符重复的标记。由于某些重复种类非常常用，因此就有一些专门用于
表示这些情况的特殊字符：   
  - {n, m}
  - {n,}
  - {n}
  - ? 等价于 {0,1}
  - \+ 等价于 {1,}
  - \* 等价于 {0, }
7. 在使用 * 和 ? 时要注意，由于这些字符可能匹配 0 个字符，因此它们允许什么都不匹配。
8. 非贪婪匹配，在待匹配的字符后面跟一个问号 +?, ??, *?, {1,5}?
9. 使用 /a+?b/ 匹配 "aaab" 时仍然会匹配整个字符串，因为正则表达式的模式匹配总会寻找字符串中的
第一个可能匹配的位置。
10. 选择项的尝试匹配次序是从左到右，直到发现了匹配项，如果左边的选择项匹配就忽略右边的匹配项，
即使它产生更好的匹配。
11. 圆括号的多种作用
  - 把单独的项组合成子表达式，以便可以像使用一个单独的单元一样对括号内的单元项进行处理
  - 在完整的模式中定义子模式
  - 允许在同一正则表达式的后部引用前面的子表达式，这是通过在字符 \ 后加一位或多位数字实现的。
  索引从 1 开始
  - 只分组不创建数字引用 (? )
12. 像 \b 这样的元素不匹配某个可见的字符，它们指定匹配发生的合法位置。有时我们称这些元素为正则
表达式的锚。
13. \B 匹配非单词边界的位置，(?=p)零宽正向先行断言，(?!p) 零宽负向先行断言
14. 在多行模式下，^ 和 $ 除了匹配整个字符串的开始和结尾之外，还能匹配每行的开始和结尾
15. String 上的模式匹配方法
  - 如果 `search()` 的参数不是正则表达式，则首先会通过 RegExp 构造函数将其转换为正则表达式，
  不支持全局搜索
  - `replace()` 不会将非正则的第一个参数转换为正则表达式，$1....
  - `match()` 会转换参数，即使 `match()` 执行的不是全局搜索，它也返回一个数组。在这种情况下，
  数组的第一个元素是匹配的字符串，余下的元素是正则表达式中用圆括号括起来的子表达式。
  - `split()`
16. 当给 `RegExp()` 传入一个字符串表述的正则表达式时，必须将 \ 替换成 \\。
17. RegExp 的属性和方法
  - source
  - global
  - ignoreCase
  - multiline
  - lastIndex
  - `exec()` 不匹配返回 null，数组的 index 属性指明了发生匹配的字符位置，input属性引用了原始
  字符串
  - `test()`    

### 13 Web 浏览器中的 JS

1. Window 对象中其中一个最重要的属性是 `document`，它引用 Document 对象，后者表示显示在
窗口中的文档
2. 在 HTML 文档里嵌入 JS 有 4 种方法：
  - 内联，放在 &lt;script&gt; 和 &lt;/script&gt; 标签之间
  - 放置在 &lt;script&gt; 标签的 src 属性指定的外部文件中
  - 放置在 HTML 事件处理程序中，该事件处理程序由 onclick 这样的 HTML 属性值指定
  - 放在一个 URL 里，这个 URL 使用了特殊的 'javascript:' 协议
3. 当浏览器遇到 type 属性值不被识别的 &lt;script&gt; 标签时，它会解析这个元素但不会尝试
显示或执行它的内容。这意味着可以使用 &lt;script&gt; 元素来嵌入任意的文本数据到文档里，只要将
type 属性声明为一个不可执行的类型
4. HTML 中定义的事件处理程序的属性可以包含任意条 js 语句，相互之间用逗号分隔。这些语句组成一个
函数体，然后这个函数成为对应事件处理程序属性的值。
5. URL 中的 javascript: 类型嵌入，这种特殊的协议类型指定 URL 内容为任意字符串，这个字符串
是会被 JS 解释器运行的代码。它被当做单独的一行代码对待，这意味着语句之间必须用分号隔开。
javascript: URL 能识别的“资源”是转换成字符串的执行代码的返回值。
6. `document.write()` 的出现是因为在最初的时候，浏览器中还没有 DOM 的 API，因此只能使用
这个函数来影响文档的内容和结构。
7. `defer` 属性使得浏览器延迟脚本的执行，知道文档的载入和解析完成，并可以操作。`async` 属性
使得浏览器可以尽快地执行脚本，而不用在下载脚本时阻塞文档解析。`async` 优先级要高一点。
8. `defer` 的脚本按出现的顺序执行，而 `async` 则可能乱序执行，
9. `document.readyState -> loading` - 文档解析完成 `document.readyState -> interactive` -
`defer` 脚本执行 - `document.DOMContentLoaded` 事件触发，注意此时可能还有异步脚本没有
执行完成 - `document.readyState -> complete` - `window.onload`
10. 怪异模式和标准模式，检查 `document.compatMode` 属性，如果为 `CSS1Compat` 说明浏览器
工作在标准模式，如果为 `BackCompat`（或 undefined），说明浏览器工作在怪异模式
11. `document.domain`，默认情况下存放的是载入文档的主机名，可以设置这一属性，不过使用的字符串
必须具有有效的域前缀或它本身。因此，如果一个 domain 属性的初始值是字符串 "home.example.com"，
就可以把它设置为字符串 "example.com"，但是不能是 "home.example" 或 "ample.com"。另外，
domain 中必须有一个点号，不能设置为 "com" 或其他顶级域名

### 14 Window 对象

1. 由于历史原因，`setTimeout()` 和 `setInterval()` 的第一个参数可以作为字符串传入，如果
这么做，那这个字符串会在指定的超时时间或间隔之后进行求值（相当于执行 `eval()`）。
2. `window.location === document.location`, `document.URL` 是文档首次载入后保存该
文档的 URL 的静态字符串。不会随着 URL 的变化而变化。
3. `location.assign()`, `location.reload()`, `location.replace()`
4. `history.back()`, `history.forward()`, `history.go()`
5. `window.navigator`, `window.screen`
6. `alert(), confirm(), prompt()` 都会产生阻塞
7. 由于历史原因，Window 对象的 `onerror` 事件处理函数的调用通过三个字符串参数，而不是通过
通常传递的一个事件对象。（其他客户端对象的 `onerror` 处理程序所需要的错误条件是不一样的，但是
它们都是正常的事件处理程序，向这个函数只需传入一个事件对象）。`window.onerror` 的第一个参数
是描述错误的一条消息。第二个参数是一个字符串，它存放引发错误的 JS 代码所在的文档的 URL。第三个
参数是文档中发生错误的行数。
8. `onerror` 处理程序的返回值也很重要，返回 false 通知浏览器事件处理程序已经处理了错误，不需要
其他操作。
9. 如果在 HTML 文档中用 id 属性来为元素命名，并且如果 Window 对象没有此名字的属性，Window
对象会赋予一个属性，它的名字是 id 属性的值，而它们的值指向表示文档元素的 HTMLElement 对象。如果
以下 HTML 元素有 name 属性的话，也会这样表现：`<a> <applet> <area> <embed> <form> <frame>`
`<frameset> <iframe> <img> <object>`。不过由于 name 属性是可以多元素共享的，因此使用
name 的情况下可能会返回类数组对象（到底一个的时候是返回一个 HTMLElement 还是类数组对象待测试）
10. 有 name 或 id 属性的 `<iframe>` 是个特殊的例子，其值为 window 对象。不过不要混淆的是
使用 `getElementById()` 等其他方法获取的还是正常的 HTMLElement 对象
11. 和相互独立的标签页不同，嵌套的浏览上下文之间并不是相互独立的，在一个窗体中运行的 JS 程序
总是可以看到它的祖先和子孙窗体。尽管可能受同源策略的影响。
12. `window.open()` 返回代表那个新窗口的 window 对象。四个参数，第一个是 URL，如果省略了，
就是 `about:blank`。第二个是新打开的窗口的名字，如果省略此参数，使用指定的名字 '_blank' 打开
一个新的、未命名的窗口，注意是未命名的。第四个是布尔值，只有在第二个参数命名的是一个已存在的窗口
时才有用，声明是要替换历史（true）还是新建条目（false，默认值）
13. Window 对象如果有 name 属性，就用它保存名字。该属性是可写的，并且脚本可以随意设置它。如果
传递给 `window.open` 一个除 '_blank' 之外的名字，通过该调用创建的窗口将以该名字作为 name
属性的初始值。如果 `<iframe>` 元素有 name 属性，表示该 iframe 的 Window 对象会用它作为
name 属性的初始值。
14. `window.opener` 属性, `window.close()`, `window.closed`
15. `self, parent, top, _top, _parent`， iframe 元素有 comtentWindow 属性，引用
该窗体的 Window 对象，表示顶级窗口的 Window 对象的 frameElement 属性为 null,窗体中的
Window 对象的 frameElement 属性不是 null。
16. `window.frames` 属性，它引用自身包含的窗口或窗体的子窗体。类数组对象，元素是 window 对象，
如果指定的 iframe 元素有 name 或 id 属性，还可以通过 name 或 id 进行索引

### 15 脚本化文档

1. DOM

![dom](https://raw.githubusercontent.com/temple-deng/markdown-images/master/uncategorized/dom.png)   

2. 为 `<form> <img> <iframe> <embed> <object>` 设置 name 属性值，也会在 Document 对象
中创建以此 name 属性值为名字的属性（当然，前提是 document 上没有对应的属性），如果给定的名字
只有一个元素，自动创建的文档属性对应的值为元素本身，否则可能是 NodeList 对象
3. `document.body, document.head, document.documentElement`
4. `document.links, document.images, document.forms` HTMLCollection 对象，可以用
name 或 id 索引。
5. NodeList 和 HTMLCollection 对象不是历史文档状态的一个静态快照，而通常是实时的。
6. `getElementsByClassName` 只需要一个字符串参数，但是该字符串可以由多个空格隔开的标识符
组成，只有当元素的 class 属性值包含所有指定的标识符才匹配。
7. `document.querySelector(), document.querySelectorAll` 的 NodeList 是非实时的。
这两个方法在 Element 上也有定义。
8. Node 定义以下重要的属性（注意是选的Node）：
  - parentNode
  - childNodes，NodeList 实时表示
  - firstChild, lastChild, nextSibling, previousSibling
  - nodeType: 9 Document, 1 Element, 3 Text, 8 Comment, 11 DocumentFragment
  - nodeValue: Text 或 Comment 节点的文本内容
  - nodeName: 元素的标签名，大写形式
9. 作为元素树的文档：
  - firstElementChild, lastElementChild
  - nextElementSibling, previousElementSibling
  - children
  - childElementCount
10. HTML 元素的属性(attribute)值在代表这些元素的 HTMLElement 对象的属性(property)中是
可用的。
11. `getAttribute(), setAttribute()` 查询和设置非标准的 HTML 属性，属性值均看做字符串，
`hasAttribute(), removeAttribute()`。
12. HTML5 在 Element 对象上定义了 dataset 属性，指代一个对象，它的各个属性对应于去掉前缀
的 data- 属性。带连字符的属性对应于驼峰命名法属性名。
13. Node 类型定义了 attributes 属性，只读的类数组对象，代表元素的所有属性，实时可使用数字索引，
也可以使用属性名索引。得到的是 Attr 对象，有 name 和 value 属性。
14. Element 的 `innerHTML, outerHTML, innerText`，Node 的 `textContent`
15. Document 的 `createElement(), createTextNode(), createDocumentFragment()`
16. Node 的 `cloneNode()` 给方法传递参数 true 能够递归地复制所有的后代节点
17. Node 的 `appendChild(), insertBefore()`, `insertBefore()` 的第一个参数是待插入的
节点，第二个是已存在的节点。如果调用这两个方法将已存在文档中的一个节点再次插入，节点将自动从它
当前位置删除并在新的位置重新插入。
18. `removeChild()`, `replaceChild()` 第一个参数是新节点。
19. 滚动条位置：   

```js
function getScrollOffsets(w) {
  w = w || window;

  if (w.pageXOffset !== null) {
    return {
      x: w.pageXOffset,
      y: w.pageYOffset
    };
  }

  var d = w.document;
  if (d.compatMode === 'CSS1Compat') {
    return {
      x: d.documentElement.scrollLeft,
      y: d.documentElement.scrollTop
    };
  }

  return {
    x: d.body.scrollLeft,
    y: d.body.scrollTop
  };
}
```  

20. 视口尺寸：   

```js
function getViewportSize(w) {
  w = w || window;

  if (w.innerWidth !== null) {
    return {
      w: w.innerWidth,
      h: w.innerHeight
    };
  }

  var d = w.document;

  if (d.compateMode === 'CSS1Compat') {
    return {
      w: d.documentElement.clientWidth,
      h: d.documentElement.clientHeight
    };
  }

  return {
    w: d.body.clientWidth,
    h: d.body.clientHeight
  };
}
```   

21. Element 的 `getBoundingClientRect()` 视口坐标，left, right, top, bottom, width,
height, 包含 padding 和 border
22. 如果想查询内联元素每个独立的矩形，`getClientRects()` 类数组对象。这两个方法都是非实时的。
23. `document.elementFromPoint()` 传递 x 和 y 坐标（视口坐标）
24. `window.scrollTo()` 和 `window.scroll()` 都是文档坐标 `window.scrollBy()` 相对
坐标
25. Element `scrollIntoView()` 接收布尔参数，false 将尽量放置到右下
26. 元素的 offset* 包含 padding 和 border，对于非定位和一些特殊元素如表格单元来说，offsetLeft,
offsetTop 是文档坐标，否则是相对于 offsetParent 的坐标。
27. clientWidth, clientHeight 不包括 border，滚动条，内联元素返回 0
28. clientLeft, clientTop 就是边框以及可能的滚动条宽度，内联返回 0
29. scrollWidth, scrollHeight 包括 padding 和溢出的宽度，scrollLeft, scrollTop 滚动条

### 16 脚本化 CSS

1. 元素的 style 属性的值为 CSSStyleDeclaration 对象，所有的值都是字符串
2. 计算样式也是一个 CSSStyleDeclaration 对象，不过是只读的，`window.getComputedStyle()`
第二个可选参数可以是伪元素字符串如 '::before'。
3. classList, DOMTokenList 对象，只读类数组对象，`add(), remove(), toggle(), contains()`
实时
4. 在脚本化样式表时，常会用到两种对象，一种是元素对象，&lt;style&gt; 和 &lt;link&gt; 元素，
第二种是 CSSStyleSheet 对象，`document.styleSheets` 属性是一个只读的类数组对象，它包含
CSSStyleSheet 对象。
5. 上述的两种对象都有一个 disabled 属性，可以开关样式表。
6. 除了直接操作样式表，还可以控制样式表中的规则。CSSStyleSheet 对象有一个 cssRules[] 数组，
它包含样式表的所有规则。每个规则是一个 CSSRule 对象。这个对象的 selectText 是规则的选择器，它引用
一个描述与选择器相关联样式的可写的 CSSStyleDeclaration 对象。
7. CSSStyleSheet 还有两个方法用来添加和删除规则 `insertRule()` 和 `deleteRule()`。     


### 17 事件处理

1. focus 和 blur 不冒泡，focusin 和 focusout 冒
2. mouseover, mouseout 冒泡，mouseenter, mouseleave 不冒
3. 当 keydown 事件产生可打印字符时，在 keydown 和 keyup 之间会产生另外一个 keypress 事件，
当按下键重复产生字符时，在 keyup 事件之前可能产生很多 keypress 事件。
4. textinput 事件，data 字符串，inputMethod
5. `preventDefault(), stopPropagation(), stopImmediatePropagation()`

### 18 脚本化 HTTP

1. `xhr.open()` 支持 GET, POST, PUT, DELETE, HEAD, OPTIONS，不区分大小写
2. 如果对相同的头调用 `setRequestHeader()` 多次，新值不会取代之前的值，相反，HTTP 请求将
包含这个头的多个副本或这个头将指定多个值。
3. `status, statusText, getResponseHeader(), getAllResponseHeaders(), responseText`
`responseXML`
4. readyState:
  - UNSENT(0): open() 未调用
  - OPENED(1): open() 已调用
  - HEADERS_RECEIVED(2): 接收到头信息
  - LOADING(3): 接收到响应主体
  - DONE(4): 响应完成
5. 如果把 false 作为第3个参数传递给 open()，就是同步的
6. `overrideMimeType()`
7. 文件类型是更通用的二进制大对象(Blob)类型中的一个子类型。XHR2 允许向 `send()` 方法传入
任何 Blob 对象。如果没有显示设置 Content-Type 头，这个 Blob 对象的 type 属性用于设置待
上传的 Content-Type 头。
8. `application/x-www-form-urlencoded, application/json, multipart/form-data`
9. 首先使用 FormData() 构造函数创建 FormData 对象，然后按需多次调用这个对象的 append()
方法把个体“部分”（可能是字符串、File 或 Blob对象）添加到请求中。当传入 FormData 对象时，
send() 会自动设置 Content-Type 头。
10. 当调用 `send()` 时，触发单个 loadstart 事件。当正在加载服务器响应时，XHR 对象会发生
progress 事件，通常每隔 50 毫秒左右。当事件完成，触发 load 事件。
11. HTTP 请求无法完成有 3 种情况，对应 3 种情况，超时触发 timeout 事件，请求终止，触发 abort
事件，还有 error 事件。
12. 对于任何请求，浏览器将只会触发 load, timeout, abort, error 中的一个，之后就触发 loadend
事件
13. 与 progress 事件相关联的事件对象，loaded 属性是目前传输的字节数值，total 属性是 Content-Length
设置的字节长度，如果不知道内容长度则为 0，如果知道内容长度则 lengthComputable 为 true，否则为
false。
14. `upload, abort(), timeout`

### 20 客户端存储

1. localStorage, sessionStorage，这两个属性都代表同一个 Storage 对象——一个持久化关联
数组，数组使用字符串来索引，存储的值也都是字符串形式的。
2. 通过 sessionStorage 存储的数据和通过 localStorage 存储的数据的有效期也是不同的：前者
的有效期和存储数据的脚本所在的最顶层窗口或者是浏览器标签页是一样的。一旦窗口或标签页被永久关闭了，
那么所有通过 sessionStorage 存储的数据也都被删除了。
3. 如果一个浏览器标签页包含两个 &lt;iframe&gt; 元素，它们所包含的文档是同源的，那么这两者
之间是可以共享 sessionStorage 的。
4. `setItem(), getItem(), removeItem(), clear(), length, key()` key() 方法传入
0～length-1 的数字，可以枚举所有存储数据的名字。
5. 无论什么时候存储在 localStorage 或 sessionStorage 的数据发生改变，浏览器都会在其他对
该数据可见的窗口对象上触发存储事件（不过，在对数据进行改变的窗口对象上是不会触发的）
6. 还要注意的是，只有当存储数据真正发生改变的时候才会触发存储事件。像给已经存在的存储项设置一个
一模一样的值，抑或是删除一个本来就不存在的存储项都是不会触发存储事件的。
7. window.onstorage 事件，事件对象相关属性：
  - key: clear()方法就是 null
  - newValue: removeItem() 是 null，但 clear() 书上并没提
  - oldValue
  - storageArea: 是 localStorage 还是 sessionStroage
  - url
8. cookie 的有效期和整个浏览器进程是一致的。   

### 22 HTML5 API

1. `navigator.geolocation.getCurrentPosition(), navigator.geolocation.watchPosition()`,
`navigator.clearWatch()`
2. 地理位置 API 是异步的，因为依赖网络之间的通信，因此上述函数是接收回调函数工作的。    

```js
navigator.geolcations.getCurrentPosition(function(pos) {
  var latitude = pos.coords.latitude;
  var longitude = pos.coords.longitude;
  var accuracy = pos.coords.accuracy;   // 精确度，以米为单位
})
```    

3. 除了第一个回调函数参数之外，`getCurrentPosition(), watchPosition()` 还接收第二个可选的
回调函数，当获取地理位置信息失败的时候，会调用该回调函数。第三个可选参数是一个配置对象，该对象的
属性指定了是否需要高精度的位置信息，该位置信息的过期时间，以及允许系统在多长时间内获取位置信息。    

```js
function whereani(elt) {
  var options = {
    enableHighAccuracy: false,

    // 如果获取缓存过的位置信息就足够的话，可以设置此属性
    // 默认值为 0，表示强制检查新的位置信息
    maximumAge: 300000,   // 5 分钟

    // 愿意等待多长时间来获取位置信息，默认为无限长
    timeout: 15000
  };
}
```    

4. `postMessage()` 方法接受两个参数，其中第一个参数是要传递的消息。该参数可以是任意基本类型值
或者可以复制的对象。第二个参数是一个字符串，指定目标窗口的源，包括协议，主机和 URL（可选的）端口
部分。
5. window.onmessage, data, source 消息源自的 window 对象（给个 window 对象干嘛？）,
origin 指定消息来源。
6. `new Worker(url)`，url 必须是同源的。worker 的 `postMessage()` 是没有目的参数的。
7. `worker.terminate()`, `WorkerGlobalScope.close()`，`importScripts()` 接受一个
或多个 URL 参数，相对地址的 URL 以传递给 Worker() 构造函数的 URL 为参照，它会按照指定的顺序
依次载入并运行这些 JS 文件。如果载入脚本的时候抛出了网络错误，或者在执行的时候抛出了错误，
那么剩下的脚本都不会载入和执行。
8. 类型化数组就是类数组对象，它和常规数组有如下重要的区别：
  - 类型化数组中的元素都是数字，使用构造函数在创建类型化数组的时候决定了数组中的数字的类型和大小
  - 类型化数组有固定的长度
  - 在创建类型化数组的时候，数组中元素总是默认初始化为 0
9. 一共 8 种类型化数组：
  - Int8Array
  - Uint8Array
  - Int16Array
  - Uint16Array
  - Int32Array
  - Uint32Array
  - Float32Array
  - Float64Array
10. 在创建一个类型化数组的时候，可以传递数组大小给构造函数，或者传递一个数组或类型化数组来初始化
数组元素。
11. set() 方法用于将一个常规或者类型化数组复制到一个类型化数组中, subarray() 用于返回部分数组
内容：   

```js
var bytes = new Uint8Array(1024);
var pattern = new Uint8Array([0,1,2,3]);
bytes.set(pattern);
bytes.set(pattern, 4);
bytes.set([0,1,2,3], 8);

var ints = new Int16Array([0,1,2,3,4,5,6,7,8,9]);
var last3 = ints.subarray(ints.length-3, ints.length);
last3[0]   // 7
```    

12. subarray() 方法不会创建数据的副本，也就是指向同一个内存区咯。
13. 类型化数组都是基本字节块的视图，称为一个 ArrayBuffer，每个类型化数组都有与基本缓冲区
相关的三个属性：   

```js
last3.buffer    // 返回一个 ArrayBuffer 对象
last3.buffer === ints.buffer   // true
last3.byteOffset   // 14 注意是 int16
last3.byteLength   // 6

// ArrayBuffer 对象自身只有一个返回它长度的属性：
last3.buffer.byteLength  // 20
```    

14. ArrayBuffer 只是 **不透明** 的字节块，可以通过类型化数组获取这些字节，但是 ArrayBuffer
自己并不是一个类型化数组。然而，要注意的是，可以像对任意 JS 对象那样，使用数字数组索引来操作
ArrayBuffer（其实是以一个普通对象的方式操作），但是，这样做并不能赋予访问缓冲区中字节的权限：   

```js
var bytes = new Uint8Array(8);
bytes[0] = 1;
bytes.buffer[0]     // undefined，这个对象上并没有键为 0 的属性
bytes.buffer[1] = 255;   // 设置普通属性
bytes.buffer[1]    // 255
bytes[1]    // 0
```   

15. 可以使用 ArrayBuffer() 构造函数创建一个 ArrayBuffer，有了 ArrayBuffer 对象后，可以
在该缓冲区上创建任意数量的类型化数组视图：   

```js
var buf = new ArrayBuffer(1024 * 1024)    // 1MB
var asbytes = new Uint8Array(buf);
var asints = new Int32Array(buf);
var lastK = new Uint8Array(buf, 1023 * 1024)  // 视最后 1KB 为字节
var ints2 = new Int32Array(buf, 1024, 256)  // 视第二个 1KB 为 256 个整数
```   

16. Blob 是不透明的，能对它们进行直接操作的就只有获取它们的大小，MIME 类型和将它们分割成更小的
Blob。   

```js
var blob = ...
blob.size    // 字节为单位
blob.type    // 如果未知的话，为 ""
var subblob = blob.slice(0, 1024, "text/plain")
var last = blob.slice(blob.size-1024, 1024)   // Blob 中最后 1KB 视为无类型
// 话说看这个意思这个 slice 第二个参数是切割的长度
```    

16. 获取 Blob:
  - Blob 支持结构性复制算法，可以通过 message 事件从其他窗口或 Worker 中获取
  - 从客户端数据获取
  - 使用 HTTP 从 XHR 中获取
  - 使用 BlobBuilder 对象从字符串、ArrayBuffer 对象以及其他的 Blob 来创建自己的 Blob。
  - File 对象
17. `window.URL.createObjectURL(), window.URL.revokeObjectURL(`,
18. `FileReader(), readAsText(blob, charset), readAsArrayBuffer(), readAsDataURL()`
`readAsBinaryString(), onload, result, readyState`
19. 目前来看需要结构性复制的地方，localStorage 和 sessionStorage，`history.pushState()`,
`history.replaceState()`, onpopstate 的 event.state, `postMessage()`


### 核心参考

1. Date  

```js
new Date()
new Date(ms)
new Date(dateString)
new Date(year, month, day, hour, minute, sec, ms)
```   

2. Date() 也可以不带 new 操作符，像一个函数一样调用。以这种方式调用时，Date()将忽略掉所有的
参数，并返回当前日期和时间的一个字符串表示。
3. 静态方法：
  - `Date.now()` 毫秒数
  - `Date.parse()` 解析一个日期即时间的字符串表示，返回其代表的毫秒数
  - `Date.UTC()` 返回 UTC 的毫秒数
4. get*:
  - `getDate()`
  - `getDay()`, 0 周日，6 周六
  - `getFullYear()`, `getHours()`, `getMilliseconds()`, `getMinutes()`, `getMonth()`
  `getSeconds()` 看来时间的获取方法都是带 s 的，时分秒毫秒
  - `getTime()`, `getTimezoneOffset()` GMT 时间与本地时间的差，用分钟表示，-480
5. set*:
  - `setFullYear(year, month, day)` 举个例子，设置日期都可以同时设置剩余的日期部分
  - `setHours(hours, minutes, secs, ms)` 设置时间也可以同时设置剩余的时间部分
6. `encodeURI()` ASCII 字母和数字以及下面的 ASCII 标点符号不会被编码：`- _ . ! * ' ()`，
由于 `encodeURI()` 的意图是编码完整的 URI，因此下面这些在 URI 中有特殊含义的 ASCII 标点
符号字符也不会被转义 `; / ? : @ & = + $ , #`。
7. uri 中的其他字符将被转换成对应的 UTF8 编码，并将结果的一、二或三个字节编码为一个 %xx 格式的
十六进制转义序列
8. `encodeURIComponent()` 就只有上面提到的字母数字和标点符号不编码，URI 中特殊含义的 ASCII
标点会被编码
9. `new Error(message)`
10. `JSON.parse(s, receiver)`: receiver 在返回解析值前，对其进行过滤或后期处理。如果
指定了 receiver 函数，该函数会为从 s 中解析的每一个原始值（不是包含这些原始值的对象或数组）
调用一次，调用 receiver 时有两个参数，第一个参数是属性名——对象的属性名或转换成字符串的数组
序号。第二个参数是对象属性或数组元素的原始值。receiver 函数的返回值会称为属性的新值，如果
返回 undefined 或者根本没有返回值，就从对象或数组中删除这个属性
11. `JSON.Stringify(o, filter, indent)` filter 可以是数组或函数，如果是 replacer 函数，
该函数会在每一个需要字符串化的值上调用。this 指向定义该值的对象或数组。replacer 函数的第一个
参数是该对象中的对象属性名或数组序号，第二个参数是值本身。replacer 函数的返回值会替换掉需要
字符串化的值。如果返回 undefined 或没有返回值，则会在字符串化时忽略掉该值。如果是字符串数组，
该数组会作为对象属性名，属性名不在该数组中的任何对象属性在字符串化时都会忽略掉。
12. String:
  - `charAt()`
  - `charCodeAt()`
  - `concat()`
  - `indexOf()`, `lastIndexOf()`
  - `localeCompare()`
  - `match()`
  - `replace()`
  - `search()`
  - `slice()`
  - `split()`
  - `substr()`
  - `substring()`
  - `toLowerCase()`, `toUpperCase()`
  - `trim()`

### 客户端 JS 参考

1. `Blob slice(start, length, [string contentType])`
2. BlobBuilder 对象用于从字符串，ArrayBuffer 对象以及其他 Blob 创建新的 Blob对象。
  - `new BlobBuilder()`
  - `void append(string text, [string endings])` 使用 utf-8 编码，话说第二个参数干嘛没介绍
  - `void append(Blob data)`
  - `void append(ArrayBuffer data)`
  - `Blob getBlob([string contentType])`
3. CSSRule，注意外链的 CSS 读取不了 cssRules 属性，会报错：
  - `cssText` 完成的 CSS 规则文本
  - `CSSRule parentRule` 如果存在的话，包含当前规则的规则
  - `CSSStyleSheet parentStyleSheet`
  - `selectorText`
  - `CSSStyleDeclaration style`
  - `type`
4. Document:
  - `Event createEvent(string eventInterface)`，创建并返回一个未初始化的合成 Event 对象，
  创建 Event 对象之后，可以对它调用一个合适的事件初始化方法来初始化它的只读属性。这些方法有
  `initEvent(), initUIEvent()` 等。创建并初始化一个合成事件对象之后，就可以使用 EventTarget
  的 `dispatchEvent()` 方法来分发它。
5. Event:
  - `void initEvent(string type, boolean bubbles, boolean cancelable)`

## JS 进阶

1. 执行上下文栈，栈底永远是全局上下文，文章上将执行上下文比喻为函数执行的环境，个人感觉这个比喻
太抽象了，与其说环境，不如说函数执行时能够访问到的各种标识符。
2. 执行上下文 EC 包括三个东西：
  - 变量对象 VO
  - 作用域链 ScopeChain
  - this

![execution-context](https://raw.githubusercontent.com/temple-deng/markdown-images/master/uncategorized/execution-context.png)   

3. 一个执行上下文的生命周期分为两个阶段：
  - 创建阶段：执行上下文创建变量对象，建立作用域链，确定 this 的指向
  - 代码执行阶段：完成变量赋值，函数引用  
  - 注意根据文章中的意思 EC 是在调用函数时才生成的  
4. 变量对象的创建，依次经历了以下几个过程
  - 建立 arguments 对象。
  - 检查当前上下文的函数声明，也就是使用 function 关键字声明的函数。在变量对象中以函数名建立
  一个属性，属性值为指向函数所在内存地址的引用。如果函数名的属性已经存在，那么该属性将会被新的
  引用所覆盖
  - 检查当前上下文中的变量声明，没找到一个变量声明，就在变量对象中以变量名建立一个属性，属性值
  为 undefined。如果该变量名的属性已经存在，为了防止同名的函数被修改为 undefined，则会直接
  跳过，原属性值不会被修改。    
5. 变量对象 VO 和活动对象 AO 都是同一个对象，只是处于执行上下文的不同生命周期，不过只有处于
函数调用栈栈顶的执行上下文中的变量对象，才会变成活动对象。
6. JS 代码的整个执行过程，分为两个阶段，代码编译阶段与代码执行阶段。编译阶段由编译器完成，将
代码翻译成可执行代码，这个阶段作用域规则会确定。执行阶段由引擎完成，主要任务是执行可执行代码，
执行上下文在这个阶段创建。
7. 作用域链是由当前环境与上层环境的一系列变量对象。
8. 柯里化是指这样一个函数，他接收函数 A 作为参数，运行后能够返回一个新的函数。并且这个新的函数
能够处理函数 A 的剩余参数。（相当于把一部分参数已经保存了起来）

## 深入理解 JS 系列

1. () 是分组操作符，只能包含表达式而不能包含语句，否则会抛出错误
2. `function foo() { /**/ }(1)` 等价于 `function foo(){ /**/ }; (1);`相当于一个
函数声明和一个没用的表达式。
3. 如果我们只是想要一个函数表达式自执行，只要将这个表达式解释为表达式而不是函数声明即可，因此，
甚至可以使用一元运算符 `!function() { /**/ }(); -function() { /**/ }();`
4. 在 ECMA 中的代码有三种类型：global, function 和 eval
5. 函数被创建时保存作用域，而调用时生成包含作用域链的 EC
6. this 会由每一次 caller 提供，caller 是通过调用表达式产生的（也就是这个函数是如何被激活
调用的）。
7. 全局上下文中的变量对象就是全局对象自己，而由于 EC 中的变量和函数声明都是变量对象的属性，也
因此，我们定义的全局变量可以作为全局对象的属性使用
8. 一个不使用 var 声明的变量是不会在变量对象上挂载的
9. this 值的首要特点是它不是静态的绑定到一个函数，this 是进入上下文时确定，在一个函数代码中，
这个值在每一次完全不同    
10. 正是调用函数的方式影响了调用的上下文中的 this 值。    

```js
function foo() {
  alert(this);
}
 
foo(); // global
 
alert(foo === foo.prototype.constructor); // true
 
// 但是同一个function的不同的调用表达式，this是不同的
 
foo.prototype.constructor(); // foo.prototype
```    

11. 引用类型：这里感觉所谓的引用类型其实指的是对象的属性或方法以及数组中的元素把。但是从下面的
代码的意思来看，所有的变量和函数的标识符都有引用类型，注意和传统的 JS 中的引用类型区分。       

使用伪代码我们可以将引用类型的值表示为拥有两个属性的对象——base（即拥有属性的那个对象），和base
中的 propertyName。   

```js
var valueOfReferenceType = {
  base: <base object>,
  propertyName: <property name>
};
```    

我们需要引用类型的值只有两种情况：   

+ 当我们处理一个标识符时（这个就单指变量或函数的情况）
+ 或一个属性访问表达式    

例如，下面标识符的值，在操作的中间结果中，引用类型对应的值如下：   

```js
var foo = 10;
function bar() {}
```    

```js
var fooReference= {
  base: global,
  propertyName: 'foo'
};

var barReference = {
  base: global,
  propertyName: 'bar'
};
```   

为了从引用类型的值中得到一个真正的值，为代码中的 GetValue 方法可以做如下描述：    

```js
function GetValue(value) {
  // 首先判断是不是一个引用类型？
  if (Type(value) != Reference) {
    return value;
  }

  var base = GetBase(value);

  if (base === null) {
    throw new ReferenceError;
  }

  // 如果是引用类型，就返回 base 上对应 property 的值？
  return base.[[Get]](GetPropertyName(value));
}
```    

内部的 [[Get]] 方法返回对象属性真正的值：   

```js
GetValue(fooReference);   // 10
GetValue(barReference);   // function object "bar"
```   

属性访问表达式的情况：    

```js
foo.bar();
foo['bar']();
```   

在中间计算的返回值中，我们有了引用类型的值：   

```js
var fooBarReference = {
  base: foo,
  propertyName: 'bar'
};

GetValue(fooBarReference);   // function object "bar"
```    

12. 一个函数上下文中确定 this 值的通用规则如下：在一个函数上下文中，this 由调用者提供，由调用
函数的方式来决定。如果调用括号 () 的左边是引用类型的值，this 将设为引用类型的 base 对象，在
其他情况下，这个值为 undefined。     
13. 非引用类型的情况：   

```js
(function() {
  alert(this);   // undefined
})();
```   

这个例子中，我们有一个函数对象但不是一个引用类型对象（excuse me?）   

```js
var foo = {
  bar: function () {
    alert(this);
  }
};
 
foo.bar(); // Reference, OK => foo
(foo.bar)(); // Reference, OK => foo
 
(foo.bar = foo.bar)(); // global
(false || foo.bar)(); // global
(foo.bar, foo.bar)(); // global
```   

在后三个例子中，赋值运算符，逻辑运算符和逗号运算符调用了 `GetValue` 方法（我们怎么知道什么时候
需要调用 `GetValue` 方法啊，难道是因为后面的例子我们确定要使用函数的值，而前两个都是需要引用
类型的值），返回的结果是函数对象（而不是引用类型）



同源策略控制不同源之间的交互，例如对 XML 和 img 元素的使用，这些交互通常可以分为3类：   

+ 跨域写是允许的。例如链接，重定向和表单提交（这都是嘛意思）。极少数 HTTP 请求需要 preflight。
+ 跨域内嵌是允许的。例子在下面。
+ 跨域读通常是不允许的，但是一些读通常使用了内嵌的方式绕过限制。例如，我们可以读取一幅内嵌图片
的宽和高。    

下面是跨域内嵌的一些例子：   

+ 外链脚本 `<script src="..."></script>`，貌似不同源的脚本的错误信息都不会显示
+ 外链 CSS `<link rel="stylesheer" href="...">`
+ 图片 `<img>`
+ 媒体文件 `<video>, <audio>`
+ 插件 `<object>, <embed>, <applet>`
+ 字体 `@font-face` 有的浏览器允许跨域字体，有的不允许
+ `<iframe>`。


## MV*

在开发应用程序的时候，以求更好的管理应用程序的复杂性，基于 **职责分离（Speration of Duties）**
的思想都会对应用程序进行分层。在开发图形界面应用程序的时候，会把管理用户界面的层次称为View，
应用程序的数据为Model（注意这里的Model指的是Domain Model，这个应用程序对需要解决的问题的
数据抽象，不包含应用的状态，可以简单理解为对象）。Model提供数据操作的接口，执行相应的业务逻辑。    

有了View和Model的分层，那么问题就来了：View如何同步Model的变更，View和Model之间如何粘合在一起。    

### Smalltalk-80 MVC

MVC出了把应用程序分成View、Model层，还额外的加了一个Controller层，它的职责为进行Model和
View之间的协作（路由、输入预处理等）的应用逻辑（application logic）；Model进行处理业务逻辑。
Model、View、Controller三个层次的依赖关系如下：    

![mvc1](https://raw.githubusercontent.com/temple-deng/markdown-images/master/uncategorized/mvc1.png)   

用户对 View 操作以后，View 捕获到这个操作，会把处理的权利交移给 Controller（Pass calls）；Controller 会对来自 View 数据进行预处理、决定调用哪个 Model 的接口；然后由 Model 执行相关
的业务逻辑；当 Model 变更了以后，会通过 **观察者模式**（Observer Pattern）通知 View；
View 通过观察者模式收到Model变更的消息以后，会向Model请求最新的数据，然后重新更新界面。如下图：   

![mvc2](https://raw.githubusercontent.com/temple-deng/markdown-images/master/uncategorized/mvc2.png)   

### MVC Model2

在Web服务端开发的时候也会接触到MVC模式，而这种MVC模式不能严格称为MVC模式。经典的MVC模式只是解决客户端图形界面应用程序的问题，而对服务端无效。服务端的MVC模式又自己特定的名字：MVC Model 2，或者叫JSP Model 2，或者直接就是Model 2 。Model 2客户端服务端的交互模式如下：

![mvc3](https://raw.githubusercontent.com/temple-deng/markdown-images/master/uncategorized/mvc3.png)   

服务端接收到来自客户端的请求，服务端通过路由规则把这个请求交由给特定的Controller进行处理，
Controller执行相应的应用逻辑，对Model进行操作，Model执行业务逻辑以后；然后用数据去渲染特定
的模版，返回给客户端。   

因为HTTP协议是单工协议并且是无状态的，服务器无法直接给客户端推送数据。除非客户端再次发起请求，
否则服务器端的Model的变更就无法告知客户端。所以可以看到经典的Smalltalk-80 MVC中Model通过
观察者模式告知View更新这一环被无情地打破，不能称为严格的MVC。    

### MVP

MVP模式把MVC模式中的Controller换成了Presenter。MVP层次之间的依赖关系如下：   

![mvc4](https://raw.githubusercontent.com/temple-deng/markdown-images/master/uncategorized/mvc4.png)   

既然View对Model的依赖被打破了，那View如何同步Model的变更？看看MVP的调用关系：   

![mvc5](https://raw.githubusercontent.com/temple-deng/markdown-images/master/uncategorized/mvc5.png)   

和MVC模式一样，用户对 View 的操作都会从 View 交移给 Presenter。Presenter 会执行相应的应用
程序逻辑，并且对 Model 进行相应的操作；而这时候 Model 执行完业务逻辑以后，也是通过观察者模式
把自己变更的消息传递出去，但是是传给 Presenter 而不是 View。Presenter获取到Model变更的
消息以后，通过View提供的接口更新界面。    

MVP 的优点：   

1. 便于测试。Presenter对View是通过接口进行，在对Presenter进行不依赖UI环境的单元测试的时候。
可以通过Mock一个View对象，这个对象只需要实现了View的接口即可。然后依赖注入到Presenter中，
单元测试的时候就可以完整的测试Presenter应用逻辑的正确性。
2. View可以进行组件化。在MVP当中，View不依赖Model。这样就可以让View从特定的业务场景中脱离出来，可以说View可以做到对业务完全无知。它只需要提供一系列接口提供给上层操作。这样就可以做到高度可复用的View组件。   

### MVVM

MVVM可以看作是一种特殊的MVP（Passive View）模式，或者说是对MVP模式的一种改良。

MVVM代表的是 Model-View-ViewModel，这里需要解释一下什么是 ViewModel。ViewModel的含义
就是 "Model of View"，视图的模型。它的含义包含了领域模型（Domain Model）和视图的状态（State）。
在图形界面应用程序当中，界面所提供的信息可能不仅仅包含应用程序的领域模型。还可能包含一些领域模型
不包含的视图状态，例如电子表格程序上需要显示当前排序的状态是顺序的还是逆序的，而这是Domain Model
所不包含的，但也是需要显示的信息。    

可以简单把ViewModel理解为页面上所显示内容的数据抽象，和Domain Model不一样，ViewModel更适合
用来描述View。   

![mvc6](https://raw.githubusercontent.com/temple-deng/markdown-images/master/uncategorized/mvc6.png)   

MVVM 的调用关系和 MVP 一样。但是，在 ViewModel 当中会有一个叫 Binder，或者是Data-binding
engine的东西。以前全部由 Presenter 负责的 View 和 Model 之间数据同步操作交由给 Binder
处理。你只需要在 View 的模版语法当中，指令式地声明 View 上的显示的内容是和 Model 的哪一块
数据绑定的。当 ViewModel 对进行 Model 更新的时候，Binder 会自动把数据更新到 View 上去，
当用户对 View 进行操作（例如表单输入），Binder 也会自动把数据更新到 Model 上去。这种方式称为：
Two-way data-binding，双向数据绑定。可以简单而不恰当地理解为一个模版引擎，但是会根据数据变更实时渲染。    


## ES6

### let, const, 解构, 字符串, 正则, 数值

1. ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数
声明语句的行为类似于 `let`，在块级作用域之外不可引用。ES6 的块级作用域允许声明函数的规则，
只在使用大括号的情况下成立，如果没有使用大括号，就会报错。   
2. `let, const, class` 声明的全局变量，不属于顶层对象的属性。
3. 本质上，解构赋值属于 “模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。    

```js
let [x, y, ...z] = ['a'];
x   // "a"
y   // undefined
z   // []
```    

4. 数组解构要求右边是可迭代的解构。
5. 如果解构赋值的默认值是一个表达式，那么这个表达式是惰性求值的。
6. 对象的解构赋值：   

```js
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz    // "aaa"
```   

7. 如果解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错：   

```js
// 报错
let {foo: {bar}} = {baz: 'baz'};
```    

8. 由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。到底是数组解构还是对象解构
是取决于等号左边的模式。
9. `\u{20BB7}`
10. `codePointAt()`    

```js
let s = '𠮷a';

s.codePointAt(0)   // 134071
s.codePointAt(1)   // 57271
s.codePointAt(2)   // 97
```    

11. `String.fromCharCode(0x20BB7) // "ஷ"`，`String.fromCharCode` 不能识别大于
0xFFFF 的码点，所以 0x20BB7 就发生了溢出，最高位 2 被舍弃了，最后返回码点 U+0BB7 的码点。
12. 解决方法就是 `String.fromCodePoint()` 方法。
13. `includes(), startsWith(), endsWith(), repeat()`, 如果 `repeat` 的参数是
负数或者 `Infinity` 报错，但是如果参数是 0 到 -1 之间的小数，则等同于 0，这是因为会先进行
取整运算。0 到 -1 之间的小数，取整以后等于 -0，NaN 也等同于 0。
14. `padStart(), padEnd()` 第一个参数用来指定字符串的最小长度，第二个参数是用来补全的字符串。
如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串。如果省略了第二个参数，默认使用
空格补全长度。
15. 模板字符串可以嵌套。
16. 标签模板：    

```js
let a = 5;
let b = 10;

tag`Hello ${a + b} world ${a * b}`;
// 等同于
tag(["Hello ", " world ", ""], 15, 50)
```    

17. `RegExp` 构造函数的一种情况是，第一个参数是一个正则表达式，这时会返回一个原有正则表达
式的拷贝，此时第二个参数指定的修饰符会覆盖掉原有的正则表达式修饰符。
18. u 修饰符：   

```js
/^\uD83D/u.test('\uD83D\uDC2A')   // false
/^\uD83D/.test('\uD83D\uDC2A')    // true
```    

19. ES6 新增了使用大括号表示 Unicode 字符，这种表示法在正则表达式中必须加上 u 修饰符，
才能识别当中的大括号，否则会被解读为量词
20. `RegExp.prototype.unicode`
21. `y` 修饰符的作用与 `g` 修饰符类似，也是全局匹配，后一次匹配都从上一次匹配成功的下一个
位置开始。不同之处在于，`g` 修饰符只要剩余位置中存在匹配就可，而 `y` 修饰符确保匹配必须从
剩余的第一个位置开始。`RegExp.prototype.sticky`
22. `RegExp.prototype.flags`
23. ES2018 引入 `s` 修饰符，使得 `.` 可以匹配任意单个字符,`dotAll`属性
24. 后行断言，`(?<=), (?<!)`
25. 具名组匹配：    

```js
const RE_DATE = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;

const matchObj = RE_DATE.exec('1999-12-31');
const year = matchObj.groups.year;   // 1999
const month = matchObj.groups.month;  // 12
const day = matchObj.groups.day;    // 31
```    

具名组匹配在圆括号内部，模式的头部添加 "问号 + 尖括号 + 组名"，然后就可以在 `exec` 方法
返回结果的 `groups` 属性上引用该组名。   

字符串替换时，使用 `$<组名>` 引用具名组。   

```js
let re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;

'2015-01-02'.replace(re, '$<day>/$<month>/$<year>');
// '02/01/2015'
```    

`replace` 方法的第二个参数也可以是函数：   

```js
'2015-01-02'.replace(re, (
  matched,    // 整个匹配结果 2015-01-02
  capture1,   // 第一个组匹配 2015
  capture2,   // 第二个组匹配 01
  capture3,   // 第二个组匹配 02
  position,   // 匹配开始的位置 0
  S,          // 原字符串 2015-01-02
  groups      // 具名组构成的一个对象 {year, month, day}
) => {
  let {day, month, year} = arguments[arguments.length - 1];
  return `${day}/${month}/${year}`;
});
```   

具名组在原来的基础上，新增了最后一个函数参数：具名组构成的一个对象。    

26. `Number.isFinite(), Number.isNaN()` 与传统的全局方法 `isFinite(), isNaN()`
的区别在于，传统方法先调用 `Number()` 将非数值转为数值，再进行判断，而这两个新方法只对
数值有效。
27. `Number.isInteger()`，如果参数不是数值，返回 `false` 不会进行转换。   

`Number.isInteger(3.0000000000000002)  // true` 这个小数的精度达到了小数点后
16 个十进制位，转成二进制位超过了 53 个二进制位，导致最后的那个 2 被丢弃了。   

28. `Number.EPSILON, Number.isSafeInteger()`
29. 指数运算符 `**`，右结合。

### 函数，数组，对象

1. 扩展运算符，操作数是一个数组，只要用于函数调用，将一个数组转为用逗号分隔的参数序列。这个东西
有点诡异啊，说是运算符，在一些简单的表达式中还不能使用，貌似只能在某些特定的语境下使用。如果扩展
运算符后面是一个空数组，则不产生任何效果 `[...[], 1] // [1]`，扩展运算符还能正确识别四个
字节的 Unicode 字符。任何实现了 Iterator 接口的对象，都可以用扩展运算符转为真正的数组。
2. `Array.from()`, 还可以接受第二个参数，作用类似于数组的 `map` 方法。也可以正确处理 Unicode
字符。
3. `Array.of()`
4. `copyWithin()` 在当前数组内部，将指定位置的成员复制到其他位置，然后返回当前数组。
5. `find(), findIndex()`, `fill()`
6. `entries(), keys(), values()` 迭代器对象
7. `includes()`
8. `flat(), flatMap()`,返回一个新数组，对源数组无影响。 `flat()` 默认只会“拉平”一层，
如果想要“拉平”多层的嵌套数组，可以将 `flat()` 方法的参数写成一个整数，表示想要拉平的层数。
甚至可以使用 Infinity 作为参数。`flatMap()` 对原数组每个成员执行一个函数，然后对返回值
组成的数组执行 `flat()` 方法，    
9. 如果对象的方法使用了 getter 和 setter，则 `name` 属性不是在该方法上面，而是在该方法
的属性描述符的 `get` 和 `set` 属性上面，返回值是方法名前加上 `get` 和 `set`:   

```js
const obj = {
  get foo() {},
  set foo(x) {}
};

obj.foo.name
// TypeError: Cannot read property 'name' of undefined
// 他这个例子有误导性，这里是因为他 foo getter 返回了 undefined，如果我们恰好
// foo getter 返回一个带有 name 属性的对象，那这里就没问题了

const desctiptor = Object.getOwnPropertyDescriptor(obj, 'foo');

descriptor.get.name  // "get foo"
descriptor.set.name  // "set foo"
```   

10. `Object.is()`
11. `Object.assign()` 合并源对象的所有自身可枚举属性。`Object.assign()` 只能进行值的
复制，如果要复制的值是一个 getter，那么将求值后再复制（怎么求值）
12. `Object.getOwnPropertyDescriptors(), Object.setPrototypeOf(obj, proto)`,
`Object.getPrototypeOf()`
13. `super` 表示原型对象。只能用在对象的方法中。
14. `Object.values(), Object.entries()`
15. 对象的扩展运算符：   

```js
let {x, y, ...z} = {x: 1, y: 2, a: 3, b: 4};
x    // 1
y    // 2
z    // { a: 3, b: 4 }
```   

16. 指定了默认值之后，函数的 `length` 属性，将返回没有指定默认值的参数个数，也就是说，指定了
默认值之后，`length` 属性将失真。同理，rest 参数也不计入。同时，如果设置了默认值的参数不是
尾参数，那么 `length` 属性也不再计入后面的参数了。
17. 如果将一个匿名函数赋值给一个变量，ES5 的 `name` 属性，会返回空字符串，而 ES6 的 `name`
属性会返回实际的函数名。但是如果将一个具名函数赋值给一个变量，则 ES5 和 ES6 的 `name` 属性
都返回这个具名函数本来的名字。`Function` 构造函数返回的函数实例，`name` 属性的值 `anonymous`。
`bind` 返回的函数，`name` 属性会加上 `bound ` 前缀

### Symbol, Set, Map, Proxy, Reflect, Promise

1. Symbol 值通过 `Symbol` 函数生成。`Symbol` 函数前不能使用 new 命令，否则会报错，
这是因为生成的 Symbol 是一个原始类型的值，不是对象。   

```js
let s = Symbol();

typeof s    // "symbol"
```

2. 注意，`Symbol` 函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的 Symbol 函数
的返回值是不等的。
3. Symbol 值不能与其他类型的值进行运算，会报错。必须显示转换为字符串或者布尔值，但不能
转换为数值。`String(s), Boolean(s), !s,  s.toString()`
那奇怪了，既然有 `toSting()` 方法为何不能参加普通运算呢。
4. Symbol 作为属性名，不会出现在 `for...in, for...of` 循环中，也不会被 `Object.keys()`
`Object.getOwnPropertyNames(), JSON.stringify()` 返回。`Object.getOwnPropertySymbols()`
可以获取指定对象的所有 Symbol 属性名。返回一个数组。`Reflect.ownKeys()` 可以返回所有类型
的键名，包含常规键名和 Symbol 键名。
5. `Symbol.for()`, `Symbol.keyFor()`
6. `Set` 本身是一个构造函数。接受可迭代结构做参数。
7. Set 实例属性，`size`, 方法 `add(value), delete(value), has(value), clear()`
add 返回 Set 结构，clear 没有返回值。
8. Set 的遍历方法，`keys(), values(), entries(), forEach()`, Set 迭代遍历顺序就是
插入顺序。   

```js
let set = new Set(["red", "green", "blue"]);

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```   

Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的 values 方法，因此可以直接用 for..of
遍历 Set。    

9. `WeakSet()` 是一个构造函数，可以使用 new 命令。三个实例方法 `add, delete, has`，没有
size 属性。
10. 作为构造函数，Map 也可以接受一个数组作为参数，该数组的成员是一个个表示键值对的数组。事实上，
任何具有 Iterator 接口，且每个成员都是一个双元素的数组的数据结构都能用作参数。
11. Map 的键将 NaN 看做与自身相等。
12. Map 的实例属性和方法 `size, set(key, value), get(key), has(key), delete(key)`
`clear(), keys(), values(), entries(), forEach()`
13. `var proxy = new Proxy(target, handler)` 注意，要使得 `Proxy` 起作用，必须针对
`Proxy` 实例进行操作，而不是针对源对象进行操作。
14. Proxy 支持的拦截操作（receiver 好像就是 proxy 自身）：
  - `get(target, propKey, receiver)` 属性读
  - `set(target, propKey, value, receiver)` 设置属性，返回一个布尔值（What？）
  - `has(target, propKey)` 拦截 `propKey in proxy` 返回一个布尔值，只拦截这一种吗？需要
  测试一下
  - `deleteProperty(target, propKey)`, 拦截 delelte 操作，返回一个布尔值
  - `ownKeys(target)`，拦截 `Object.getOwnPropertyNames(proxy), Object.getOwnProperySymbols(proxy)`
  `Object.keys(proxy), for...in` 循环，返回一个数组
  - `getOwnPropertyDescriptor(target, propKey)`，返回属性描述符
  - `defineProperty(target, propKey, descriptor)` 返回布尔值，包括 `defineProperties()`
  方法，那问题就来啦，如果是 `defineProperties` 方法，参数是怎样的。
  - `preventExtensions(target)` 返回布尔值
  - `getPrototypeOf(target)` 返回对象
  - `isExtensible(target)` 布尔
  - `setPrototypeOf(target, proto)` 布尔
  - 如果原对象是函数，还有额外两种操作可以拦截：
  - `apply(target, object, args)` 拦截 Proxy 实例作为函数调用的操作，比如 `proxy(...args)`
  `proxy.call(object, ...args), proxy.apply(object, args)`
  - `constructor(target, args)`。
15. Reflect 上的静态方法基本对应 Proxy, receiver 主要是用来在 setter 和 getter 中绑定
到 this。   

```js
var myObject = {
  foo: 1,
  bar: 2,
  get baz() {
    return this.foo + this.bar;
  },
};

var myReceiverObject = {
  foo: 4,
  bar: 4,
};

Reflect.get(myObject, 'baz', myReceiverObject) // 8
```   

貌似所有 Reflect 上的方法如果 target 不是对象，就抛出错误了，不会进行多余的转换，而 Object
上的方法可能还会进行隐式的转换。    

16. 注意，调用 resolve 或 reject 并不会终结 Promise 的参数函数的执行，也即是说没有 return
效果咯。   

```js
new Promise((resolve, reject) => {
  resolve(1);
  console.log(2);
}).then(r => {
  console.log(r);
});
// 2
// 1
```   

17. 如果 Promise 状态已经变成 resolved，再抛出错误是无效的。    

```js
const promise = new Promise(function(resolve, reject) {
  resolve('ok');
  throw new Error('test');
});

promise.then(value => {
  console.log(value);
}).catch(err => {
  console.log(err);
});
// ok
```    

上面代码中，Promise 在 `resolve` 语句后面，再抛出错误，不会被捕获，等于没有抛出。因为
Promise 的状态一旦改变，就永久保持该状态，不会再变了。   

18. `Promise.prototype.finally()`，`finally` 方法的回调函数不接受任何参数。
19. `Promise.all()`, Iterator 接口，Promise 实例
20. `Promise.race()`

### Iterator, Generator, async

1. 每一次调用 `next` 方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含
`value` 和 `done` 两个属性的对象。其中，`value` 属性是当前成员的值，`done` 属性是一个
布尔值，表示遍历是否结束。
2. ES6 规定，默认的 Iterator 接口部署在数据结构的 `Symbol.iterator` 属性。原生具备
Iterator 接口的数据结构如下：
  - Array
  - Map
  - Set
  - String
  - TypedArray
  - Arguments 对象实例
  - NodeList 对象实例
3. 调用 Iterator 的场合：
  - `for...of` 循环
  - 解构赋值
  - 扩展运算符
  - `yield*`
  - `Array.from()`
  - `Map(), Set(), WeakMap(), WeakSet()`
  - `Promise.race(), Promise.all()`
4. 遍历器对象的 `return(), throw()` 方法。
5. `function` 关键字和函数名之间有一个星号。`yield` 表达式是暂停标志。
6. `yield` 表达式如果用在另一个表达式之中，必须放在圆括号里。
7. yield 表达式本身没有返回值，或者说总是返回 `undefined`。`next` 方法可以带一个参数，
该参数就会被当做上一个 yield 表达式的返回值。
8. for...of 循环一旦遍历到返回对象 `done: true` 时就会终止，且不包含该对象。因此，return
语句的值通常不会遍历到。其他利用迭代器接口的地方也是。
9. `throw` 方法抛出的错误要被内部捕获，前提是必须至少执行过一次 `next` 方法。`throw` 方法
被捕获之后，会附带执行下一条 `yield` 表达式。也就是说，会附带执行一次 `next` 方法。
10. `Generator.prototype.return()`。如果 Generator 函数内部有 try...finally 代码块，
那么 `return()` 方法会推迟到 `finally` 代码块执行完再执行。
11. `yield*` 可迭代结构，如果被代理的 Generator 函数有 return 语句，那么就可以向代理它
的 Generator 函数返回数据：    

```js
function* foo() {
  yield 2;
  yield 3;
  return 'foo';
}

function* bar() {
  yield 1;
  var v = yield* foo();
  console.log("v: " + v);
  yield 4;
}

var it = bar();

it.next();
// { value: 1, done: false }
it.next();
// { value: 2, done: false }
it.next()
// { value: 3, done: false }
it.next()
// "v: foo"
// { value: 4, done: false }
it.next()
// { value: undefined, done: true }
```   

12. 正常情况下，`await` 命令后面是一个 Promise 对象，如果不是就返回对应的值。

### Class, Decorator, Module

1. 类的内部所有定义的方法，都是不可枚举的。
2. 类和模块的内部，默认就是严格模式。
3. 类必须使用 `new` 调用，否则会报错。这是它和普通构造函数的一个主要区别。
4. 与函数一样，类也可以使用表达式的形式定义。   

```js
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};
```    

采用 Class 表达式，可以写出立即执行的 Class：   

```js
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');
```   

5. 类不存在变量提升。
6. 如果在一个方法前，加上 `static` 关键字，就表示该方法不会被实例继承，而是直接通过类来
调用，这就称为“静态方法”。父类的静态方法，可以被子类继承。
7. 类的实例属性可以用等式，写入类的定义之中。类的静态属性只要在实例属性写法前面，加上 `static`
关键字即可。
8. `new.target` 一般用在构造函数之中，返回 `new` 命令作用于的那个构造函数。如果构造函数
不是通过 `new` 命令调用的。`new.target` 会返回 `undefined`。
9. 子类必须在 `constructor` 方法中调用 `super` 方法，否则新建实例时会报错。如果子类没有定义
`constructor` 方法，这个方法会被默认添加，代码如下。也就是说，不管有没有显示定义，任何一个
子类都有 `constructor` 方法。   

```js
class ColorPoint extends Point {
}

// 等同于

class ColorPoint extends Point {
  constructor(...args) {
    super(...args);
  }
}
```   

10. `Object.getPrototypeOf()` 方法可以用来从子类上获取父类。`Object.getPrototypeOf(ColorPoint) === Point // true`    
11. 修饰器对类的行为的改变，是代码编译时发生的，而不是在运行时。
12. 方法修饰器可以接受三个参数：   

```js
function readonly(target, name, descriptor) {
  descriptor.writable = false;
  return descriptor
}
```    

第一个参数是类的原型对象，第二个参数是要修饰的属性名，第三个参数是该属性的描述符。    

13. ES6 模块不是对象，而是通过 `export` 命令显示指定输出的代码，再通过 `import` 命令输入。
14. 通常情况下，`export` 输出的变量就是本来的名字，但是可以使用 `as` 关键字重命名：   

```js
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```    

15. `import` 命令输入的变量都是只读的，因为它的本质是输入接口，也就说，不允许在加载模块的
脚本里面，改写接口。   
16. 由于 `import` 是静态执行，所以不能使用表达式和变量。`import` 语句会执行所加载的模块。
17. 加载默认接口，指定加载接口，整体加载。
18. 正是因为 `export default` 命令其实是输出一个叫做 `default` 的变量，所以它后面不能
跟变量声明语句。    

```js
// 正确
export var a = 1;

// 正确
var a = 1;
export default a;

// 错误
export default var a = 1;
```    

同样地，因为 `export default` 命令的本质是将后面的值，赋给 `default` 变量，所以可以直接将
一个值写在 `export default` 之后。   

```js
// 正确
export default 42;

// 报错
export 42;
```   

19. `import()` 加载模块成功以后，这个模块会作为一个对象，当作 `then` 方法的参数。
20. 浏览器对于带有 `type="module"` 的 `<script>`，都是异步加载，不会造成阻塞浏览器，等同
于开启了 `defer` 属性。
21. Node 要求 ES6 模块采用 `.mjs` 后缀文件名，也就是说，只要脚本文件里面使用 `import` 或者
`export` 命令，那么就必须采用 `.mjs` 后缀名。`require` 命令不能加载 `.mjs` 文件，只有
`import` 命令才可以加载 `.mjs` 文件。反过来，`.mjs` 文件里面也不能使用 `require` 命令，
必须使用 `import`。    
22. Node 规定 ES6 模块之中不能使用 CJS 模块特有的一些内部变量，以下顶层变量在 ES6 模块之中
是不存在的：   
  - `arguments`
  - `require`
  - `module`
  - `exports`
  - `__filename`
  - `__dirname`    
23. CJS 模块的输出都定义在 `module.exports` 这个属性上面。Node 的 `import` 命令加载
CJS 模块，Node 会自动将 `module.exports` 属性，当作模块的默认输出。   
24. 由于 ES6 模块是编译时确定输出接口，CJS 模块是运行时确定输出接口，所以采用 import 命令
加载 CJS 模块时，不允许采用下面的写法 `import { readFile } from 'fs'`。因为，fs 是
CJS 格式，只有在运行时才能确定 `readFile` 接口，而 `import` 命令要求编译时就确定这个接口。
25. CJS 模块加载 ES6 模块，不能使用 `require` 命令，而要使用 `import()` 函数。ES6 模块
的所有输出接口，会成为输入对象的属性。