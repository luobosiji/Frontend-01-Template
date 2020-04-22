# 产生式(BNF 巴克斯诺尔范式)

定义一门语言 只能由 a 和b 来组成：

"a"  终结符

"b"  终结符

<Program> 非终结符

程序最高的结构
<Program>:= "a"+ | "b"+
BNF可递归 构成如下：
<Program>:= <Program> "a"+ | <Program> "b"+


- 定义加法 允许连加：

<Number> = "0" | "1" | "2" | "3" | ...... | "9"
十进制数
<DecimalNumber> = "0" | (("1" | "2" | "3" | ...... | "9") <Number>* )
两个十进制数相加
<AdditiveExpression> = <DecimalNumber> "+" <DecimalNumber> 
引入递归，可以连加
<AdditiveExpression> = <AdditiveExpression> "+" <DecimalNumber>
合并成 加法表达式 （单个数字或者 连加）
<AdditiveExpression> = <DecimalNumber> | <AdditiveExpression> "+" <DecimalNumber>

四则运算：1 + 2 * 3

重要的（括号）：
<PrimaryExpression> = <DecimalNumber> |
    "(" <LogicalExpression> ")"
乘除：
<MultiplicativeExpression> = <PrimaryExpression> | 
  <MultiplicativeExpression> "*" <PrimaryExpression> |
  <MultiplicativeExpression> "/" <PrimaryExpression> 
加减：
<AdditiveExpression> = <MultiplicativeExpression> | 
  <AdditiveExpression> "+" <MultiplicativeExpression> |
  <AdditiveExpression> "-" <MultiplicativeExpression> 
逻辑运算：
<LogicalExpression> = <AdditiveExpression> | 
  <LogicalExpression> "||" <AdditiveExpression> |
  <LogicalExpression> "&&" <AdditiveExpression> 
