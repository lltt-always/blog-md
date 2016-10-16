---
title: hash算法
tags: 算法
---

简述hash算法的概念与特性

<!--more-->

# 定义

hash算法：将任意长度的二进制值（明文）映射为较短的固定长度的二进制值（hash值）。

# 优秀hash算法的特性

- 正向快速：给定明文和hash算法，在有限时间和有限资源内能计算出hash值。
- 逆向困难：给定hash值，在有限时间内基本不可能推出明文。
- 输入敏感：原始输入信息修改一点，产生的hash有很大不同
- 冲突避免：很难找到hash值相同的两段内容不同的明文。

# 流行算法

- MD4
  
  输出为128位；

- MD5
 
  比MD4复杂，计算速度略慢，但相对安全，输出为128位；

- SHA-1

  输出为160位，原理与MD4相同

- SHA-2

  原理跟SHA-1类似，安全性更高。

# 参考资料

- [hash算法简介](https://yeasy.gitbooks.io/blockchain_guide/content/crypto/hash.html)
