title: JWT 认证
author: Light Fish
date: 2019-06-04 18:39:23
tags:
---
# JWT (JSON Web Token 认证机制)

> JSON Web Token (JWT)是一个开放标准(RFC 7519)，它定义了一种紧凑的、自包含的方式，用于作为JSON对象在各方之间安全地传输信息。该信息可以被验证和信任，因为它是数字签名的。这个也是JWT可能较为官方的定义，在我前几篇博客中也是简单介绍过token的应用场景，作为前后端分离的时候的身份验证，其加解密的方法也是较为简单，这次就来正式的介绍下token的正确用法——JWT

<!-- more -->

> 网上也是有着很多的教程，但是基本都是鉴于DRF框架使用的，所以这次就是介绍下JWT的用法，不涉及到DRF框架。

## PyJWT 介绍

> emmm。没错，这个就是这次介绍的猪脚——pyjwt。一个Python现成的第三方库，安装就用`pip`

```python
pip install pyjwt
```

### 利用pyjwt 生成token （实例介绍JWT）

```
import jwt
from datetime import datetime, timedelta

token = jwt.encode({'exp':datetime.utcnow()+timedelta(seconds=30),
					'iat':datetime.utcnow(),
					'data':{'username':'lightfish'}},
					'secret_key',
					algorithm='HS256')

out>>  b'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1NTk2Mjc5NjIsImlhdCI6MTU1OTYyNzkzMiwiZGF0YSI6eyJ1c2VybmFtZSI6ImxpZ2h0ZmlzaCJ9fQ.SyGMyOxwzu9mswJDpPZKEv5F8MzYBxMLXixhxtMHh6k'

```

> 如上就是jwt的最基本的加密过程，生成的结构是有三部分组成 `header`.`payload`.`signature`

* header 

> header典型的由两部分组成：token的类型（“JWT”）和算法名称（比如：HMAC SHA256或者RSA等等）,如下是可以通过base64解码得出

![img](http://qnpic.top/2019-06-04%5C1.png)

* Payload

> JWT的第二部分是payload，它包含声明（要求）。声明是关于实体(通常是用户)和其他数据的声明。声明有三种类型: registered, public 和 private。
>
> * Registered claims : 这里有一组预定义的声明，它们不是强制的，但是推荐。比如：`iss` (issuer jwt 签发者), `exp` (expiration time 过期时间 如上我所定义的), `sub` (subject, jwt所面向的用户), `aud` (audience,接收jwt的一方),`iat`:(jwt的签发时间), `nbf`(定义在什么时间之前，该jwt都是不可用的)等。这里的话，你可能主要用到的就是exp和iat，过期时间和签发时间
> * Public claims : 可以随意定义。
> * Private claims : 用于在同意使用它们的各方之间共享信息，并且不是注册的或公开的声明。

如上我所定义的

![img](http://qnpic.top/2019-06-04%5C2.png)



**注意**这里最后加=的原因是base64解码对传入的参数长度不是2的对象，需要再参数最后加上一个或两个等号=

> 因为JWT不会对结果进行加密，所以不要保存敏感信息在Header或者Payload中，服务端也主要依靠最后的Signature来验证Token是否有效以及有无被篡改

* Signature

> 第三部分Signature是用header+payload+secret_key进行加密的结果,这个`secret_key`也就是JWT安全的地方，其是放在服务器的秘钥

### 解密token

```
jwt.decode(token, 'secret_key', algorithms=['HS256'])
```

![img](http://qnpic.top/2019-06-04%5C3.png)

> **注意**,这里也是注意下，我前面也是讲过JWT的Registered claims(预定义声明),而我的token你也是可以看到我是token设置了30秒，这里在解密的时候就会用到预声明哦～～

![img](http://qnpic.top/2019-06-04%5C4.png)

> 另外。我前文也是说过JWT并没有对数据进行加密，如果没有secret_key也可以直接获取到Payload里边的数据，只是缺少了签名算法无法验证数据是否准确，pyjwt也提供了直接获取Payload数据的方法，如下

```
jwt.decode(token, verify=False)

out: {'exp': 1559629462, 'iat': 1559629432, 'data': {'username': 'lightfish'}}
```

## 应用场景

> 以上就是JWT 的介绍了，这里我就在大致介绍下在Django 开发中的简单应用，你可以去写个装饰器，或者一个拦截器，大致方法如下

```python
try:
	auth = request.META.get('HTTP_AUTHORIZATION')
except AttributeError:
	return JsonResponse({"status": 402, "message": "No authenticate header"})

try:
	dict = jwt.decode(auth, settings.SECRET_KEY, algorithms=['HS256'])
	username = dict.get('data').get('username')
except jwt.ExpiredSignatureError:
	return JsonResponse({"status": 401, "message": "Token expired"})
except jwt.InvalidTokenError:
	return JsonResponse({"status": 401, "message": "Invalid token"})
except Exception as e:
	return JsonResponse({"status": 401, "message": "Can not get user object"})
	
try:
	user = User.objects.get(username=username)
except:
	return JsonResponse({"status_code": 401, "message": "User Does not exist"})
```

> 以上就是后端对token的基本验证，用户验证、token过期验证和token错误验证，至于如何给用户token，emmm，登录的时候吧。或者在User用户表里添加一个静态方法

```python
    @property
    def token(self):
        return self._generate_jwt_token()

    def _generate_jwt_token(self):
        token = jwt.encode({
            'exp': datetime.utcnow() + timedelta(days=1),
            'iat': datetime.utcnow(),
            'data': {
                'username': self.username
            }
        }, settings.SECRET_KEY, algorithm='HS256')

        return token.decode('utf-8')
```

### 最终效果图

> 像这种过期时间和秘钥尽可能定义在settings文件里

![img](http://qnpic.top/2019-06-04%5CGIF.gif)

<br><br>

以上就是对JWT的基本介绍和使用，So,<br><br>Just have fun

