# Unicode与UTF家族

### Unicode

Unicode是一种字符集，其中编码了很多的字符，每个字符都有唯一的编号，即“码点”（codepoint）。

码点的范围目前是0-0x10ffff，一个21比特的无符号整型，但是在计算机处理中大多都会把它直接当成一个32位无符号整型。每一个字符都有一个码点，但不是每一个0-0x10ffff的整数都是一个有效的码点。

Unicode是向下兼容ASCII的，这意味着ASCII中的字符在Unicode中的码点就是它们的ASCII码。

可以用Javascript中的`codePointAt`函数来获取一个字符的码点：

```javascript
"a".codePointAt(0).toString(16)
```

结果为61，就是字符“a”的ASCII编码。

用同样的方法，可以知道汉字“好”的码点，为0x597d。

反过来，在Javascript中也可以用`String.fromCodePoint`从码点获取字符

```javascript
String.fromCodePoint(0x597d)
```

结果为“好”。

码点可用U+C的形式表示，C是码点的十六进制，用最少四位数来表示。如“a”的Unicode就是：U+0061，“好”的Unicode是：U+597D。

Unicode只是为每个字符编了码，但是那些字符是怎么进行存储的呢？这就需要用到UTF（Unicode Transformation Format）编码。

### UTF-32

UTF-32算是一种最简单粗暴的编码方式了，直接就是把每个码点当成32位无符号整型，然后存储它们的二进制，不做其他的转换。由名字也可知，UTF-32存储的每一个字符占用4字节空间，是一种定长的编码方案。

比如“🍎”的码点是127822，补全到32位的二进制是：

<span style="color: pink">00000000 0000000</span>11 1110011 01001110

以大端模式在内存中的存储是：00 01 F3 4E，那么这个就是UTF-32BE的编码结果了。

但是由于一个32位整型是占4个字节，也就是需要超过一个字节来编码一个32位整型，于是这里就有了字节序（Endianness）的问题。

因此UTF-32分大小端，分别写作UTF-32BE和UTF-32LE。



可以使用位操作进行编码：

```c
char dest[4];
uint32_t c = 127822;
dest[0] = (char) (c & 0xFF);
dest[1] = (char) ((c >> 8) & 0xFF);
dest[2] = (char) ((c >> 16> & 0xFF));
dest[3] = (char) ((c >> 24) & 0xFF);
```

`dest`就是UTF-32BE的编码结果。



不过也能偷个懒，直接用C语言中的指针把32位无符号整型的内存表示取出来，此时UTF-32编码使用的字节序就是自己机器的字节序。

```c
uint32_t c = 127822;
char *p = ((char *) &c);
```

`p`指针处偏移为0，长度为4的数据就是编码结果。

### UTF-8

UTF-8是一种可变长的编码方案，每一个字符编码0 - 4个字节。[<sup>nb1</sup>](#nb1)

| Codepoint range    | Byte 1   | Byte 2   | Byte 3   | Byte 4   |
| ------------------ | -------- | -------- | -------- | -------- |
| U+0000 - U+007F    | 0xxxxxxx |          |          |          |
| U+0080 - U+07FF    | 110xxxxx | 10xxxxxx |          |          |
| U+0800 - U+FFFF    | 1110xxxx | 10xxxxxx | 10xxxxxx |          |
| U+10000 - U+10FFFF | 11110xxx | 10xxxxxx | 10xxxxxx | 10xxxxxx |

算法也不难，先根据码点范围确定字节长度，然后把码点的二进制对表格中相应行里的“x”从后往前填充就可以了，不够的补零。

例如："🍎"的码点是0x1f341，二进制是：1 11110011 01001110，根据表格前面的范围可知编码成UTF-8后需要四个字节。现在用它的二进制从后往前填充表格中的空：

原二进制：<span style="color: blue">1 1111</span><span style="color: green">0011 01</span><span style="color: red">001110</span>

填充：11110<span style="color: pink">000</span> 10<span style="color: pink">0</span><span style="color: blue">11111</span> 10<span style="color: green">001101</span> 10<span style="color: red">001110</span>

填充结果就是所得的UTF-8编码了，所以“🍎”的UTF-8编码为：F0 9F 8D 8E

在解析UTF-8的时候就可以根据最头一个字节判断那个字符被编码的字节长，然后根据相应长度的编码方式逆回去就好了。

### UTF-16

UTF-16也是一种变长的编码方案，每个字符可以被编码成2或4个字节。由于一个需要把一个16整型拆成两个字节，所以UTF-16也存在字节序的区分，分别记为：UTF-16BE，UTF-16LE。

- 对于范围在U+0000 - U+FFFF的码点，UTF-16是用两个字节来编码，此时也是简单粗暴，编码结果直接就是码点的二进制。

  如“好”的码点是22909，二进制为：

  <span style="color: pink">0</span>1011001 01111101

  同UTF-32，用同样的方式就可以得到UTF-16BE的编码结果：59 7D

- 但是如果码点范围在U+FFFF - U+10FFFF呢？

  此时UTF-16就需要4个字节来编码了。那么如何保证在编码过后能知道一个字符是占两字节还是四字节呢，这肯定得有一些标记，因此UTF-16里面引入了一个叫代理对（Surrogate Pair）的东西，作为码点超过U+FFFF的映射。

  Unicode中划分了两块区域，就是为了编码UTF-16，充当高位代理和低位代理。

  | Block           | Name                        |
  | :-------------- | --------------------------- |
  | U+D800 - U+DB7F | High Surrogates             |
  | U+DB80 - U+DBFF | High Private Use Surrogates |
  | U+DC00 - U+DFFF | Low Surrogates              |
  
  U+DB80 - U+DBFF的范围是用于私有区块的，这里直接就把它们统称为高位代理，也就是：

  | Codepoint range | Usage           |
  | :-------------- | --------------- |
  | U+D800 - U+DBFF | High Surrogates |
  | U+DC00 - U+DFFF | Low Surrogates  |
  
  由于一个21位的码点用16位整型存不下，所以依靠代理码点，算法如下：

  把要代理的码点减去10000<sub>16</sub>，此时的值就限定在了20位。

  - 把20位的结果值高10位加上d800<sub>16</sub>，即高位代理的开始码点，就是高位代理结果了。

  - 把20位的结果值低10位加上dc00<sub>16</sub>，即低位代理的开始码点，就是低位代理结果了。

    例如“🍎”的码点为127822，减去10000<sub>16</sub>得62286，转换为二进制是<span style="color: red;">0000111100</span> <span style="color: green">1101001110</span>
  
    高10位<span style="color: red;">0000111100</span>加上d800<sub>16</sub>得d83c<sub>16</sub>
    
    低10位<span style="color: green">1101001110</span>加上dc00<sub>16</sub>得d34e<sub>16</sub>
    
    最终得到“🍎”的代理对是{0xd83c, 0xd34e}，然后分别对高位代理和低位代理UTF-16编码，就得到了最终4字节的UTF-16编码。上面例子的UTF-16BE编码是：D8 3C D3 4E



### 批注

- 1. <span id="nb1"></span> nb1: UTF-8编码可能上升至6字节，但是为了兼容UTF-16的最大编码，依然设置为4字节。来自[RFC2279](https://tools.ietf.org/html/rfc2279)的改变。

