[toc]
# 字符串编码表示字符
一个字符有不同的表示方法，比如
- `'\xST'`
  - 作为一个字符，16进制表示，`hexadecimal`或`hex`，其中`S`和`T`分别都是在`0123456789abcdefABCDEF`范围内
- `'\0ST'`
  - 作为一个字符，8进制表示，`octal`，其中`S`和`T`分别都是在`01234567`范围内

# base64和ASCII的转换

## 浏览器端

  浏览器端提供了`atob`、`btoa`方法
  - [btoa](https://developer.mozilla.org/en-US/docs/Web/API/btoa)
    - `window.btoa(str)`，其中str为`binary string`，即二进制数据
    - `binary -> ascii`
    - 字符串str编码为base64 
  - atob
    - `window.atob(str)`
    - `ascii -> binary`
    - base64解码为字符串

### 针对unicode的btoa
```js
// convert a Unicode string to a string in which
// each 16-bit unit occupies only one byte
function toBinary(string) {
  const codeUnits = new Uint16Array(string.length);
  for (let i = 0; i < codeUnits.length; i++) {
    codeUnits[i] = string.charCodeAt(i);
  }
  const charCodes = new Uint8Array(codeUnits.buffer);
  let result = '';
  for (let i = 0; i < charCodes.byteLength; i++) {
    result += String.fromCharCode(charCodes[i]);
  }
  return result;
}

// a string that contains characters occupying > 1 byte
const myString = "☸☹☺☻☼☾☿";

const converted = toBinary(myString);
const encoded = btoa(converted);
console.log(encoded);                 // OCY5JjomOyY8Jj4mPyY=

```

```js
function fromBinary(binary) {
  const bytes = new Uint8Array(binary.length);
  for (let i = 0; i < bytes.length; i++) {
    bytes[i] = binary.charCodeAt(i);
  }
  const charCodes = new Uint16Array(bytes.buffer);
  let result = '';
  for (let i = 0; i < charCodes.length; i++) {
    result += String.fromCharCode(charCodes[i]);
  }
  return result;
}

const decoded = atob(encoded);
const original = fromBinary(decoded);
console.log(original);                // ☸☹☺☻☼☾☿

```
## NodeJs

NodeJs没有`atob`、`btoa`方法，可通过全局的Buffer模块生成Base64
```node
Buffer.from('admin:password').toString('base64');
```

# unicode

## 表示范围

### unicode表示范围`0~0x10FFFF`
### UTF-16表示范围`0~0xFFFF`

## 函数
- charCodeAt(pos)
  - 返回一个UTF-16单元
- charPointAt(pos)
  - 若pos指定位置没元素，则返回`undefined`
  - 若pos指定位置为UTF-16的高阶部分，则返回代理对对应的代码点
  - 若pos指定位置为UTF-16的低阶部分，则返回低阶部分的代码点

[示例](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/codePointAt)
```js
'ABC'.codePointAt(0)                        // 65
'ABC'.codePointAt(0).toString(16)           // 41

'😍'.codePointAt(0)                         // 128525
'\ud83d\ude0d'.codePointAt(0)               // 128525
'\ud83d\ude0d'.codePointAt(0).toString(16)  // 1f60d

'😍'.codePointAt(1)                         // 56845
'\ud83d\ude0d'.codePointAt(1)               // 56845
'\ud83d\ude0d'.codePointAt(1).toString(16)  // de0d

'ABC'.codePointAt(42)                       // undefined

```
# unicode和ASCII的转换

**在多数浏览器中，使用 btoa() 对 Unicode 字符串进行编码都会触发 InvalidCharacterError 异常。**

```js
// ucs-2 string to base64 encoded ascii
function utoa(str) {
    return window.btoa(unescape(encodeURIComponent(str)));
}
// base64 encoded ascii to ucs-2 string
function atou(str) {
    return decodeURIComponent(escape(window.atob(str)));
}
// Usage:
utoa('✓ à la mode'); // 4pyTIMOgIGxhIG1vZGU=
atou('4pyTIMOgIGxhIG1vZGU='); // "✓ à la mode"

utoa('I \u2661 Unicode!'); // SSDimaEgVW5pY29kZSE=
atou('SSDimaEgVW5pY29kZSE='); // "I ♡ Unicode!"

```

# encodeURIComponent vs encodeURI
## 区别
它们都是编码URL，唯一区别就是编码的字符范围，其中
- `encodeURI`方法不会对下列字符编码  `ASCII字母`  `数字`  `~!@#$&*()=:/,;?+'`
- `encodeURIComponent`方法不会对下列字符编码 `ASCII字母`  `数字`  `~!*()'`
  
所以encodeURIComponent比encodeURI编码的范围更大。

## 使用场景
1. 如果只是编码字符串，不和URL有半毛钱关系，那么用escape。
2. 如果你需要编码整个URL，然后需要使用这个URL，那么用encodeURI。比如
  `encodeURI("http://www.cnblogs.com/season-huang/some other thing");`
  编码后会变为
  `"http://www.cnblogs.com/season-huang/some%20other%20thing";`
  其中，空格被编码成了%20。但是如果你用了encodeURIComponent，那么结果变为`"http%3A%2F%2Fwww.cnblogs.com%2Fseason-huang%2Fsome%20other%20thing"`
  看到了区别吗，连 "/" 都被编码了，整个URL已经没法用了。
3. 当你需要编码URL中的参数的时候，那么encodeURIComponent是最好方法。
  ```js
    var param = "http://www.cnblogs.com/season-huang/"; //param为参数
    param = encodeURIComponent(param);
    var url = "http://www.cnblogs.com?next=" + param;
  ```