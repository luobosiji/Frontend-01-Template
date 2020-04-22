# 每周总结可以写在这里

# 编程语言通识

## 语言按语法分类

### 非形式语言
- 中文、英文
- C++ 
  
### 形式语言（乔姆斯基谱系 定义语言）
> 基于BNF理解形式语言的种类
- 产生式(BNF 巴克斯诺尔范式)[这里是演变过程](/document/02.1-BNF.md)
	- 用尖括号括起来的名称来表示语法结构名
	- 语法结构分成基础结构和需要用其它语法结构定义的复合结构
		- 基础结构称**终结符** * 符号 (symbol)
			- Number、+ - * /
		- 复合结构称**非终结符** x * y（Expression）
	- 引号和中间的字符表示终结符（如 字符串）
	- 可以有括号
	- \* 表示重复多次
	- | 表示 或
	- \+ 表示至少一次
	- 定义加法
	- 四则运算

- 0型 无限制文法 `?::=?`
  - 等号左边和右边可以有多个非终结符
  - `<a> <b> ::= "c"`
- 1型 上下文相关文法
  - 一个词根据位置不同意思也不同 跟上下文相关
  - `?<a>?::=?<b>?`
  ```javascript
  只有中间可以变 上下文是固定的
  类似于下边
    "x" 在 "a""c" 之间被解释成 <b>
    "a" <b> "c" ::= "a" "x" "c"
    或者：
    "```javascript
    //xxxxx
    ```"
  ```
- 2型 上下文无关文法(javascript属于2型)
	- `<a>::=?`
  	- 一个词放到哪都是一个意思
  	- 等号左边是固定的：
  	- 不管右边是什么 都产生固定的一个非终结符

- 3型 正则文法
	- `<a>::=<a>?`
  	- 左递归 a出现在最左 则后边只能出现在最左 
    - 错误示例：`<a>::=?<a>`
- EBNF ABNF Coustomized
  - JavaScript 是自定义产生式[参考BNF演变过程](/document/02.1-BNF.md)
    - : 代替 :=
    - 换行代替 |
    - 终结符用加粗来表示
    - ECMA262 (Grammar Summary)
      - Lexical Grammar 词法分析
      	- 词法定义都是用双冒号 ::
      - 语法定义 是用单冒号 :
```javascript
// 这里是 JavaScript 产生式
  // AdditiveExpression:
  //   MultiplicativeExpression
  //   AdditiveExpression +(加粗) MultiplicativeExpression
  //   AdditiveExpression -(加粗) MultiplicativeExpression
```

## 图灵完备性
> 跟图灵机等效的 都具有图灵完备性
> 
> 图灵完全性通常指“具有无限存储能力的通用物理机器或编程语言”。
- 命令式——图灵机
  - 一种将人的计算行为抽象掉的数学逻辑机，其更抽象的意义为一种计算模型，可以看作等价于任何有限逻辑数学过程的终极强大逻辑机器。
  - goto
  - if和while（分支和循环）
    - 有 分支和循环的 都具有图灵完备性
- 声明式——lambda
  - 递归

## 动态与静态
- 动态
  - 在用户设备、在先服务器上
  - 产品实际运行时
  - Runtime
- 静态
  - 在程序员设备上
  - 产品开发时
  - Compiletime

## 类型系统
- 动态系统与静态类型系统
- 强类型与弱类型（有隐式转换的是弱类型）
  - String + Number
  - String == Boolean 
    - 先转化为number 在转化为字符串 跟String比较
- 复合类型
  - 结构体
    - 对象
  - 函数签名
    - 函数参数
    - 函数返回值
- 子类型
  - 逆变/协变
    - 协变：凡是能用父类型的地方 都能用子类型
    - 逆变：凡是能用子类型的地方 都能用父类型
    - 继承

## 一般命令式编程语言
- Atom
  - Identifier（标识符）
  - Literal（字面量）
- Expression 表达式
  - Atom
  - Operator（操作）
  - Punctuator （标点符号 辅助运算符）
- Statement 语句
  - Expression
  - Keyword
  - Punctuator
- Structure 结构化程序设计
  - Function
  - Class
  - Process
  - Namespace
- Program 程序集
  - Program
  - Module
  - Package
  - Library

# JavaScript 词法&类型

## A Grammar Summary
### A.1 Lexical Grammar
```javascript
// 定义 源字符 是任何 unicode 代码点
  SourceCharacter ::
    any Unicode code point
//  Unicode字符集：字符集合 规定了一系列字符
//  字符 对应 码点（正整数 ）
```
#### Unicode（基本是 U+0000 四位，后边引入Emoji位数不够变成五位）
>（0-128）ascii字符(美国信息交换标准代码)常用部分 兼容性强
>
> `'a'.codePointAt(0)` 可以查看`a`字符对应的码点
> 
> `String.fromCharCode(num)` 可以查看码点对应的字符
> `String.fromCodePoint`  超出四位使用这个
- [Unicode官网](https://home.unicode.org/)
- [Unicode查阅文档fileformat](http://www.fileformat.info/info/unicode/)
  - blocks
    - basic latin 
      - List without images (fast)
    - CJK Unified Ideographs 
      - C中文 J日文 K韩文
      - U+4E00	U+9FFF 范围判断中文
        - 因为有好多增补的字符 所以有时候用unicode判断并不准确
    - Specials 往上	
      - 四位能表示的范围（BMP 基本字符平面）兼容性特别好
      - 超出四位 使用以下API
        - `String.fromCodePoint`
        - `String.codePointAt`
  - Categories 分类
    - Separator, Space 空格
  - 中文变量名
    - 因涉及到文件的编码保存方式，使用 `\u十六进制unicode`转译（`'厉'.codePointAt(0).toString(16)`）
  ```javascript
    // 这里 \u5389\u5bb3 与 厉害 等效
    var \u5389\u5bb3 = 2
    console.log(厉害) // 输出2
  ```
  
#### InputElement
- WhiteSpace 空格
  - `<TAB>`
  - `<VT>` 纵向制表符  \v
  - `<FF>` FORM FEED
  - `<SP>` 普通空格 0020
  - `<NBSP>` NO-BREAK SPACE  `&nbsp;`解决排版问题，有空格但是不会从中间折行
  - `<ZWNBSP>` U+FEFF  ZERO WIDTH NO BREAK SPACE
    FEFF 另一个名字 BOM BIT ORDER MASK（第一个字符 有可能永远按照 Bom 处理 吞一个字符 所以 前端规范 页首加一个回车）
  - `<USP>`
- LineTerminator 换行符
  - `<LF>` \n LINE FEED（统一使用这个）
  - `<CR> `回车 CARRIAGE RENTURN
  - 超出 unicode 之外
    - `<LS>` 分行符 LINE SEPARATOR
    - `<PS>` 分段符 PARAGRAPH SEPARATOR
- Comment 注释
- CommonToken
- Punctuator: 符号 比如 `> = < }`
- Keywords：比如 `await`、`break`... 不能用作变量名，但像 getter 里的 `get`就是个例外
  - Future reserved Keywords: `eum`
- IdentifierName：标识符，可以以字母、_ 或者 $ 开头，代码中用来标识**[变量](https://developer.mozilla.org/en-US/docs/Glossary/variable)、[函数](https://developer.mozilla.org/en-US/docs/Glossary/function)、或[属性](https://developer.mozilla.org/en-US/docs/Glossary/property)**的字符序列
  - 变量名：不能用 Keywords
  - 属性：可以用 Keywords

##### Literal: 直接量
  - Number
    - 存储 Uint8Array、Float64Array
    - 各种进制的写法
      - 二进制0b
      - 八进制0o
      - 十六进制0x
    - 实践
      - 比较浮点是否相等：Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON
      - 如何快捷查看一个数字的二进制：(97).toString(2)
  - String
    - Character
    - Code Point
    - Encoding
      - unicode编码 - utf
        - utf-8 可变长度 （控制位的用处）
    - Grammar
      - `''`、`""`、``` `
  - Boolean
  - Null
  - Undefind


## 知识补充
### ASCII 码
- 计算机内部，所有信息最终都是一个**二进制值**。每一个二进制位（bit）有0和1两种状态，因此**八个二进制位**就可以组合出256种状态，这**被称为一个字节（byte）**。也就是说，一个字节一共可以用来表示256种不同的状态，每一个状态对应一个符号，就是256个符号，从00000000到11111111。
- 上个世纪60年代，美国制定了一套字符编码，**对英语字符与二进制位之间的关系**，做了统一规定。这被称为 ASCII 码，一直沿用至今。
- ASCII 码**一共规定了128个字符的编码**，比如空格SPACE是32（二进制00100000），大写的字母A是65（二进制01000001）。这128个符号（**包括32个**不能打印出来的**控制符号**），只占用了一个字节的后面7位，最前面的一位统一规定为0。

### Unicode
>将世界上所有的符号都纳入其中。每一个符号都给予一个独一无二的编码，那么乱码问题就会消失。这是一种所有符号的编码。
- Unicode 只是一个符号集，它只规定了符号的二进制代码，却没有规定这个二进制代码应该如何存储。
- 两个严重的问题
  - **如何才能区别 Unicode 和 ASCII** ？计算机怎么知道三个字节表示一个符号，而不是分别表示三个符号呢？
  - 英文字母只用一个字节表示就够了，如果 Unicode 统一规定，每个符号用三个或四个字节表示，那么每个英文字母前都必然有二到三个字节是0，这**对于存储来说是极大的浪费**，文本文件的大小会因此大出二三倍，这是无法接受的。

### UTF-8
> 互联网的普及，强烈要求出现一种统一的编码方式。UTF-8 就是在互联网上使用最广的一种 Unicode 的实现方式。
> 
> **UTF-8 是 Unicode 的实现方式之一**
- 它是一种变长的编码方式。它可以使用1~4个字节表示一个符号，**根据不同的符号而变化字节长度**。
- 编码规则
  - 对于单字节的符号，字节的第一位设为0，后面7位为这个符号的 Unicode 码。因此对于英语字母，UTF-8 编码和 ASCII 码是相同的。
  - 对于n字节的符号（n > 1），第一个字节的前n位都设为1，第n + 1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的 Unicode 码。
    - 从最后一个二进制位开始，（二进制）依次从后向前填入

```javascript
// 规则
Unicode符号范围     |        UTF-8编码方式
(十六进制)        |              （二进制）
----------------------+---------------------------------------------
0000 0000-0000 007F | 0xxxxxxx
0000 0080-0000 07FF | 110xxxxx 10xxxxxx
0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
// 以汉字严为例，演示如何实现 UTF-8 编码。
// 严的 Unicode 是4E25（100111000100101），
// 根据上表，可以发现4E25处在第三行的范围内（0000 0800 - 0000 FFFF），
// 因此严的 UTF-8 编码需要三个字节，即格式是1110xxxx 10xxxxxx 10xxxxxx。
// 然后，从严的最后一个二进制位开始，依次从后向前填入格式中的x，多出的位补0。
// 这样就得到了，严的 UTF-8 编码是11100100 10111000 10100101，
// 转换成十六进制就是E4B8A5。
```