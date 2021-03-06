---
title: Bitcoin的密码学基础
prev: ./00 Introduction.md
next: ./02 Data Structure used in Bitcoin.md 
---

# Bitcoin的密码学基础

## 哈希函数
哈希函数，将原文通过一系列的处理，最终映射为某一比特串，我们把这一比特串称之为哈希值。哈希函数是一类函数，其中常见的有：`MD5`、`SHA256`等等。同一哈希函数，尽管输入的原文大小不尽相同，但是其输出的哈希值有一致的大小，如`SHA256`把任何长度的原文都映射为长度为256的哈希值。

### 符号表示
`H(p) = s`，其中`p`表示原文，`s`表示哈希值，`H`表示某一哈希函数

### 性质
1. collision resistance  
碰撞：原文`x`、`y`不同，但`H(x) = H(y)`，则称`x`与`y`发生碰撞。
抗碰撞性：不容易找到不同的原文`x`和`y`，他们的哈希值相同，即`H(x) = H(y)`。
抗碰撞性的言下之意是理论上肯定存在碰撞，因为如果肯定不存在碰撞，那么就没有必要谈抗不抗碰撞。这一点也很好证明，因为哈希值长度是有限的，所以哈希函数的输出空间是有限的，而显然输入空间却是无限的，由抽屉原理，是肯定存在碰撞的。
2. hiding  
已知某一原文`x`的哈希值`H(x)`，是很难确定`x`的，哈希函数单向不可逆，哈希值隐藏起了原文。
3. puzzle friendly  
已知某一原文`x`的哈希值`H(x)`的范围，同样很难找出满足条件的`x`，只能通过暴力（`BF`）方法遍历求解满足条件的`x`。

### 在Bitcoin中的应用
Proof of work、挖矿

## 非对称加密
密钥对：(公钥`u`，私钥`r`)  
公钥可以公开，而私钥必须保密存储。能满足这一点，是因为通过公钥推出其对应的私钥是几乎不可能的。
  
### 符号表示
`U(p) = c`  
`R(p) = s`  
`R(U(p)) = R(c) = p = U(s) = U(R(p))`   
其中，`U`表示使用公钥进行加密/签名验证，`R`则表示使用私钥进行解密/签名。`p`、`c`、`s`分别表示原文 (pliantext)、密文 (ciphertext) 和对原文的签名 (signature)

### 作用
加密与身份验证。  
- 加密
假设通讯双方是`A`和`B`，`A`想要给`B`发消息，那么`A`可以使用`B`的公钥来进行加密，再将得到的密文发送给`B`，而`B`用其私钥来解密即可。
- 身份验证
假设一报文通过某私钥进行签名，如果对于签名的结果，能用`A`的公钥来验证解开，则说明该报文是由`A`的私钥签名的，即报文是由`A`发出的，假设`A`的私钥并没有泄露。其理论原因在于，几乎不可能存在两个不同的人但有相同的密钥对的情况。或者，两人的密钥对不同，但对于某报文，用其中一个人的私钥来签名而却能用另外一个人的公钥解开的情况也是几乎不可能发生的。

### 在Bitcoin中的应用
对交易信息进行签名，确保不可否认性，防止身份伪造。  
虽然，Bitcoin是加密货币 (Cryptocurrency ) 的一种，但是其并不是利用到非对称加密体系的加密功能，因为所有交易的记录都是公开的。其反而是利用到非对称加密体系的身份验证功能来确保不可否认性。

## 补充
- `MD5`，是`Message Digset 5`的缩写。山东大学[王小云](https://baike.baidu.com/item/王小云/29050?fr=aladdin)教授提出了密码哈希函数的碰撞攻击理论，即模差分比特分析法，提高了破解了包括`MD5`在内的5个国际通用哈希函数算法的概率。  
- `SHA256`，是`SHA`（`Secure Hash Algorithm`）哈希函数家族中的一种。  
- 抗碰撞性，还具体分为强抗碰撞性、弱抗碰撞性。