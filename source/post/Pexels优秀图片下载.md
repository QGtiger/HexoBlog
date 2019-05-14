title: Pexels优秀图片下载
author: Lightfish
tags:
  - Python
  - spider
categories:
  - 爬虫
date: 2018-12-20 20:51:00
---
# Pexels优秀图片下载

>在日常生活中，相信大家一定有过没有好的，有内涵的图片而烦恼，比如，做ppt的时候,没有好看的图片做背景；想换壁纸的时候也是没有好看、清新的图片做壁纸，今天我就用Python写一个爬虫，智能爬取优质图片。爬取的网站是[pexels](https://www.pexels.com/)

<!-- more -->

## 有道翻译API

>因为该网站是不支持中文输入的，所以，免得你们(wo)总是往返于翻译软件,所以我就直接又去写了个翻译API，这里我就感谢[有道翻译](http://fanyi.youdao.com/)的友情赞助(为什么要有道呢，因为有道简单)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx16f475j31gc0gpabd.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx16jkgej31h20oln3w.jpg)

>上图一就是我们熟悉的翻译操作，打开开发者调试，右键 检查，或者直接按`F12`进入(记得刷新哦~)。点击Network-All，就可以看见我们图二的数据，是一个`POST`请求，下面那个`From Data`就是我们发送的data，所以，算了，no bb show you the code

```
# 下面这些就是网站那复制下来构造的，虽然现在略有改动，应该也差不多，你也可以试试自己构造
data = {
    'i': '风景', # 默认我加了风景，你也可以不加，但是要有i这个参数
    'from': 'AUTO', # 你要翻译的,AUTO就是自适应
    'to': 'en', # 翻译成什么语言，emm，按理是，但是依旧一样，还是百度翻译好
    'smartresult': 'dict', # 返回类型 
    'client': 'fanyideskweb',
    'salt': '1537680464627',
    'sign': 'c72a93599c0c533050645cbe45bfd391',
    'doctype': 'json',
    'version': 2.1,
    'keyfrom': 'fanyi.web',
    'action': 'FY_BY_REALTIME',
    'typoResult': 'false'
}
# 请求头
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'
}
def YouDao():
    content = input('请输入你要翻译的单词或语句：')
    data['i'] = content
    html = requests.post(
        'http://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule',
        data=data,
        headers=headers)
    html = json.loads(html.text)
    print('翻译：'+html['translateResult'][0][0]['tgt'])
    return html['translateResult'][0][0]['tgt']
```

>**注意**，下面这个是疑难点，前年20分大题，去年没考，今年必考啊~~
>就是你开发者模式下的访问地址是`http://fanyi.youdao.com/translate_o?smartresult=dict&smartresult=rule`,但是你用这个去发POST请求是错的，要删了这个`_o`

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx16mm7dj310a0j1djj.jpg)

## Pexels网站的图片地址构造

>首先，我们先进入pexels的[网站](https://www.pexels.com/),输入你想要搜索的图片类型，如我搜索的是night，然后就是进入了night的搜索页面，这里你就能看见搜索页面的网址，就是原网站加"/search/night"或者你搜索的单词，这里就是你要构造的网址。emmmm，好像有点偏远了，我本来这一步不是讲这个的，算了。

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx16q871j31hb0peqa9.jpg)

>因为这个网站是异步加载的，即使根据用户使用动态加载了，所以想要更多的获取它的图片，我们要分析它，获取它图片数据的由来途径。先保持在开发者调试下，不断往下翻，这个时候相信你一定看见了，当我们往下翻的时候，浏览器就会接受xhr数据，如下图

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx16v35dj31h00pkaif.jpg)

>上面就是动态加载的数据来源地址，你去浏览器直接访问，就会发现，就是接着原来网页的下面，所以，而且像`&format=js&seed=2018-12-20%2001:54:27%20+0000`这种数据其实是可以删除的，不相信你可以删除再访问，是不是是一样的。这样就更加利于我们构造url地址

## 图片地址的获取

>相信到这里，你一定直接获取过它的html数据，没有我就来展示一下

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx16zejlj31000oedox.jpg)

>别忘了`import requests`和像个请求头，没有是访问不到的。由于这样不明显，我就在网页上进行适当的分析，点击下图的左上角红框，然后选中网页上的一个图片，开发者调试界面就会出现你点击图片的属性等等，我们的图片地址就是在其中，比如在`data-big-src`属性内，你可以双击选中地址，浏览器直接访问查看，就是你点击的图片

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx176w0aj31h10phdzw.jpg)

>So,接下来就是不断的分析了，查看每个图片的关联，这里我用的是`BeautifulSoup`,你也可以用正则、`xpath`等等,因为这里我发现每个`img`的class都是`photo-item__img`。

>相信到这里你们都已经会了，下面就是代码的一些展示

```
"""
author:lightfish
Time:2018.11.18
note:下载pexels图片
"""
import requests
import json
import os
from bs4 import BeautifulSoup
import math

data = {
    'i': '风景',
    'from': 'AUTO',
    'to': 'en',
    'smartresult': 'dict',
    'client': 'fanyideskweb',
    'salt': '1537680464627',
    'sign': 'c72a93599c0c533050645cbe45bfd391',
    'doctype': 'json',
    'version': 2.1,
    'keyfrom': 'fanyi.web',
    'action': 'FY_BY_REALTIME',
    'typoResult': 'false'
}
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'
}


def YouDao():
    content = input('请输入你要的图片类型：')
    data['i'] = content
    html = requests.post(
        'http://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule',
        data=data,
        headers=headers)
    html = json.loads(html.text)
    print('翻译：'+html['translateResult'][0][0]['tgt'])
    return html['translateResult'][0][0]['tgt']


def get_page_urls(url):
    try:
        html = requests.get(url, headers=headers).text
        soup = BeautifulSoup(html, 'html.parser')
        imgs = soup.find_all('img', attrs={'class': 'photo-item__img'})
        list = []
        for img in imgs:
            list.append(img.get('data-big-src'))
        return list
    except Exception as e:
        print(e)


def downloadPic(name, pic_url, localPath, n):
    path = localPath + '/' + name + '/'
    if not os.path.exists(path): 
        os.mkdir(path)
    for i, url in enumerate(pic_url[:n]):
        try:
            i = i + 1
            pic = requests.get(url, headers=headers)
            print(path + name + str(i) + '.jpg')
            with open(path + name + str(i) + '.jpg', 'wb') as f:
                f.write(pic.content)
            print('loading {} pic...'.format(str(i)))
        except Exception as e:
            print('something error')
            print(e)
            continue


def getPexelPic(search_word):
    url = 'https://www.pexels.com/search/'
    pic_num = input('请输出您要爬取的图片数(阿拉伯数字)：')
    while True:
        if pic_num.isdigit():
            break
        else:
            pic_num = input(
                'I said just input a number! :')
    page = math.ceil(int(pic_num)/30)
    url = url + search_word + '/'
    pic_urls = []
    page = int(page) + 1
    for x in range(1, int(page)):
        re_url = url + '?page=' + str(x)
        print(re_url)
        pic_urls.extend(get_page_urls(re_url))

    downloadPic(search_word, pic_urls, 'E:/PythonPic',int(pic_num))


if __name__ == '__main__':
    search_word = YouDao()
    getPexelPic(search_word)
```

>成功的截图

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx17fc3hj30sz0fcwhz.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx17kdodg30fa0jbdgd.jpg)

<br><br><br>由于是国外的网站，访问速度确实有点慢，而且有时候会断，emmm，可以重新来一遍，就好了。下次我写个百度图片的爬取，因为这个网站如果你想搜索具体的图片可能不行，当然你也可以对我的代码进行适当的扩展，如下载到本地的路径可以自己输入So<br><br>Just for fun...