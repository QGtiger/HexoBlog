title: Python实现免费发短信
author: Lightfish
tags:
  - Python
  - twilio
categories:
  - Python
date: 2019-01-04 13:22:00
---
# twilio实现免费发短信

>一直对这方面蛮感兴趣的，比如wxpy模块，登录微信。但是依旧有着小小的缺陷，所以，今天我要讲的是twilio模块实现免费发送短信，可以配合爬虫实现定时的小功能，比如，定时每天给你要发送的对象发天气预报什么的，emmm，这些就不是今天所讲的范围内了。

<!-- more -->

## twilio 模块的安装

>安装`twilio`就和一般的安装一样，用`pip`， `pip install twilio`即可安装。

![twilio](https://ws1.sinaimg.cn/large/006bO2RVly1fyuhqsnz6oj30ro0fztfv.jpg)

>当然也会安装别的依赖库。。

## 注册账号

>1.安装好库，就需要带官网进行注册（PS：这里需要翻墙，毕竟是国外的网站）。这里我再重新注册一遍,这里就是填写你的一些信息，很简单，我就不多说了。

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fyuhwvpzm3j31h70qejtf.jpg)

>2.注册完登录后，就是基本如下图。值得注意的是，`ACCOUNT SID`和`AUTH TOKEN`这两个参数是等会会用的。

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fyuhut2ynmj31hc0pstej.jpg)

>3.登录后，要想发短信，当然要有发送方咯，所以我们要去获取它。第一次应该是没有的。所以你要去注册一个，放心是免费的

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fyui2h6deuj31gw0qldk0.jpg)

>4.有了发送方，就要有接受方了。因为这个接收方是要验证的，所以并不可以进行什么短信轰炸。。点击下图的加号，就可以进行验证

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fyui61xcx5j31gt0pqacq.jpg)

>5.点击加号后，会出现下一图，它是语音验证，你想进行短信验证就点击下`text you instead.`,记得修改你的国家地区，然后输入你要发送人的手机号码进行验证，会发一个验证码给你输入的手机号。

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fyuiaawq75j30h407uwew.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fyuiacol0yj30h107j3yw.jpg)

>6.**究极注意**，你一定要去设置了进行设置中国地区，否则就是下面这个错误，国内网上教程都没有讲这里。

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fyuiirgiruj31fi08g0vx.jpg)

>7.所以你要去[这里](https://www.twilio.com/console/sms/settings/geo-permissions)进行设置中国，默认我是没有的。我的界面左边那个图片是没有的，你也可以直接输入网址: https://www.twilio.com/console/sms/settings

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fyuilcwg6nj31h70qntdf.jpg)

## 代码部分

>代码部分就很简单，如下：

```
from twilio.rest import Client

# 这里的account和token就是你前面登录主页的那两个
account = 'Your ACCOUNT SID'
token = 'Your AUTH TOKEN'

client = Client(account,token)

message = client.messages.create(
    from_='Your number', #这里写你前面注册给你的手机号，上注册账号的第三步
    to='+86*******', #这里就是你前面验证的发送对象，上注册账号的第四步
    body='Hello, I\'m from Python...' #这里就是你要发送的信息
)

print(message.sid)
```

截图如下：

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fyuisad62mj30zm0khack.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fyuitvr8svj30yi1pcn2a.jpg)

emmm,上面两条是我之前测试的。

>依然值得注意的就是注册账号中的第六、七步，异地要设置自己的国家哦~

<br><br><br>
这次的博客就到这里了，So<br><br>Just Have Fun...


