## 作业
- 写一个正则表达式 匹配所有 Number 直接量
```javascript
  NumericLiteral :: 
    DecimalLiteral  // 十进制
    BinaryIntegerLiteral // 二进制
    OctalIntegerLiteral // 八进制
    HexIntegerLiteral // 十六进制
```
```javascript
// DecimalLiteral
// DecimalIntegerLiteral
/^0|[1-9]\d*$/
// .DecimalIntegerLiteral
// DecimalIntegerLiteral.DecimalDigits
/^\.\d+|(0|[1-9]\d*)\.?\d*$/
// DecimalIntegerLiteral ExponentPart(ExponentIndicator SignedInteger)
/^(\.\d+|(0|[1-9]\d*)\.?\d*)([eE][\+\-]?\d+)?$/
// BinaryIntegerLiteral
/^0[bB][01]+$/
// OctalIntegerLiteral
/^0[oO][0-7]+$/
// HexIntegerLiteral
/^0[xX][0-9a-fA-F]+$/

// NumericLiteral
/^(\.\d+|(0|[1-9]\d*)\.?\d*)([eE][\+\-]?\d+)?$|^0[bB][01]+$|^0[oO][0-7]+$|^0[xX][0-9a-fA-F]+$/

```
- 写一个 UTF-8 Encoding 的函数

```javascript
  function customEncoding(str){
    var back = [];
    for (var i = 0; i < str.length; i++) {
      var code = str.charCodeAt(i);
      if (0x00 <= code && code <= 0x7f) {
        // 0xxxxxxx
        back.push(code);
      } else if (0x80 <= code && code <= 0x7ff) {
        // 110xxxxx 10xxxxxx
        back.push((192 | (31 & (code >> 6))));
        back.push((128 | (63 & code)))
      } else if ((0x800 <= code && code <= 0xd7ff) 
          || (0xe000 <= code && code <= 0xffff)) {
        // 1110xxxx 10xxxxxx 10xxxxxx
        back.push((224 | (15 & (code >> 12))));
        back.push((128 | (63 & (code >> 6))));
        back.push((128 | (63 & code)))
      }
    }
    for (i = 0; i < back.length; i++) {
      // 只保留低八位的数
      back[i] &= 0xff;
      back[i] = back[i].toString(16)
    }
    return back.join('')
  }
```

- 写一个正则表达式，匹配所有的字符串直接量，单引号和双引号