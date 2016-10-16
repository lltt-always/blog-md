---
title: JSON Web Token (JWT)
tags: Javascript
---

简述 JSON Web Token

<!--more-->

# JWT

JWT 是一种基于 token 的认证方案，允许我们使用JWT在用户和服务器之间传递安全可靠的信息。
一个 JWT token 看起来是这样的：

`eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6ImNoZXhpYW5nIn0.VIeJERHwnB7_YL5iF0umrRu-EeYvrdD521rjYv4NKy4`

它的结构可以简化为：
`base64url_encode(Header) + '.' + base64url_encode(Claims) + '.' + base64url_encode(Signature)`

# Header

包含一些元数据，至少表明 token 类型和签名方法。

```
{
    "type": "jwt", //token 类型为 JSON Web Token
    "alg": "HS256" //token 所使用的签名算法
}
```

# Claims(Payload)

存放[JWT 标准](https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32)定义的一些信息，也可以包含其他任何JSON兼容的字段。如下所示，

```
{
    "iss": "John Wu JWT", //JWT发布的委托人
    "iat": 1441593502, //JWT签发时间
    "exp": 1441594722, //JWT过期时间
    "aud": "www.example.com", //JWT的制定对象
    "sub": "jrocket@example.com", //作为JWT主体的委托人
    "jti": , //针对当前token的唯一标识
    "nbf": , //不能在该时间节点前接受处理JWT
    "from_user": "B",  //其他字段
    "target_user": "A" //其他字段
}
```

# Signature

JWT 标准遵照 [JWS标准](https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32#ref-JWS)来生成签名，签名主要用于token是否有效，是否被篡改。[签名算法](https://tools.ietf.org/html/rfc7518#section-3.1)

```
content = base64url_encode(Header) + '.' + base64url_encode(Claims)
signature = hmacsha256.hash(content)
```

**说到这里有一点需要特别注意的是，默认情况下，JWT 中信息都是明文的，即 Claims 的内容并没有 被加密，可以通过 base64url_decode(text) 的方式解码得到 Claims 。 所以，不要在 Claims 里包含敏感信息，如果一定要包含敏感信息的话，记得先将 Claims 的内容进行加密（比如，使用 JSON Web Encryption (JWE) 标准进行加密） 然后在进行 base64url_encode 操作。**

# 参考文件

- []()