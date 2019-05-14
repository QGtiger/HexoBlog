title: VIP音乐免费下载
author: Lightfish
tags:
  - python
  - spider
categories:
  - 爬虫
date: 2018-12-20 16:02:00
---
# VIP音乐免费下载

>好久没有更新博客了，今天我就来写一个爬虫吧。如果你是那种喜欢缓存歌，喜欢听歌的人，你一定遇到过这种情况，`由于版权原因无法播放(缓存)此歌曲`，或者是`请使用客户端下载`(emmm,有些下载客户端也不能下载)。所以，为了世界的幸福感，为了守护世界的和平，贯彻爱与真实的邪恶，可爱又迷人的反派角色，emmm，对不起跑题了，今天我就写一个，突破这种限制的小小小音乐爬虫，So Just have fun again...

<!-- more -->

就拿杰伦的这首稻香，当你点击播放的时候，就显示下面这个界面，无法播放

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx4qkemaj30w20pstd5.jpg)

所以有了这篇博客的由来。

### 网页分析

>先拿一首能播放的音乐，就拿这首薛之谦的《刚刚好》来分析，进入这个页面右键`检查`,或者按`F12`进入开发者调试界面，再选中`Network`,你应该就能看到下面这个界面(如果不能，你就刷新页面)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx4qof0cj31g40on45d.jpg)

上面那个歌曲链接，如果你直接去访问应该是就直接下载了，但是这样对稻香这种歌，根本不能播放，就不会去接受数据播放音乐，也就是说不会有这个url链接

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx4qu0blj31g60lztez.jpg)

**但是**，莫慌，慢慢来

>我们先来看看它是如何接受这首歌的地址，我们先你前面选中的`Media`换成`All`,然后刷新页面，这个时候你肯定看到乱七八糟的'东西'，如下

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx4r7s0aj31fz0ocn76.jpg)

>这个时候不能脑壳疼，这些只是你刷新页面后接受的数据，我们要的东西一定就是在这里面，所以，我们就要进行筛选，这里我就简单说一种，你点击`size`,让它从大到小排序。。。这个时候，你应该就更能看清了，

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx4rd82tj30vp0j079g.jpg)

>第一个数据那么大，你点击进去发现，就是前面我们找到的歌曲url，显然不是我们现在要的数据。所以，我们接着往下找，一个个点击进去，并点`Preview`(这样看更加直观)，分别是一个个图片，直到点到这个

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx4qylxdj31gr0phakg.jpg)

>点`Preview`，我们就能更加肯定是我们要找的数据包

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx4r3fc7j31ev0obdrj.jpg)

>把前面的URL地址复制下来直接去访问看看，发现就是我们要的数据(（。－_－。），总算找到了)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx4rm4dvj31gq0qc12r.jpg)

>找到了以后，这次的博客我们就可以写代码了

### 代码部分

>思路已经讲清，就简单展示下我们的code。因为每个人的写代码思路不同就不详述了

```
"""
author:lightfish
time:2018.12.19
note:千千音乐的批量下载
"""
import requests
import re
import json


def get_music_resource(songid):
    """
    :param songid: 歌曲的id
    :return: 因为我们爬取的数据并不是规范的json格式数据，所以我们就得进行适当的处理，让他变成
    规范的json格式数据，这里，我用的是正则方法
    """
    search_url = 'http://musicapi.taihe.com/v1/restserver/ting?method=baidu.ting.song.playAAC&format=jsonp&callback=jQuery172047648654448286276_1545221906467&songid={}'.format(
        songid)
    response = requests.get(search_url).text
    res = re.findall(r'\((.*)\)', response)[0]
    res_json = json.loads(res)
    return res_json


def get_music_info(jsondata):
    songinfo = jsondata['songinfo']
    music_title = songinfo['title']
    print('歌名: ' + music_title)
    music_compose = songinfo['compose']
    print('作者: ' + music_compose)
    album_title = songinfo['album_title']
    print('专辑: ' + album_title)
    avatar = songinfo['artist_list'][0]['avatar_s300'] if songinfo['artist_list'][0]['avatar_s300'] else ''
    print('头像: ' + avatar)
    music_language = songinfo['language'] if songinfo['language'] else ''
    print('语种: ' + music_language)
    music_country = songinfo['country'] if songinfo['country'] else ''
    print('国家: ' + music_country)
    music_url = jsondata['bitrate']['file_link']
    print(music_url)
    return music_title, music_url


def music_download(filename, url):
    with open(filename + '.mp3', 'wb') as f:
        f.write(requests.get(url).content)


if __name__ == '__main__':
    songid = input('请输入歌曲的id: ')
    data = get_music_resource(songid)
    music_title, music_url = get_music_info(data)
    is_download = input('是否下载(y/n): ')
    if is_download.lower() == 'y':
        music_download(music_title, music_url)
        print('下载完成，Just for fun...')
    elif is_download.lower() == 'n':
        print('Just for fun...')
    else:
        print('emmm ,exiting...')

```

### 成功截图

>emmm,忽然发现都快忘了我们周董的《稻香》了，其实是一样的，也有着songid，你去访问也是能获得你想要的链接地址。anywhere，Just for fun...

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx4rragsj31gm0qf11s.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywx4rvc5dj31gr0qd48l.jpg)


<br><br><br>以上就是这次博客的一点小小内容了，你可以进行设当的扩展，如只需要输入歌手的名称就下载全部该歌手的音乐(emmm,本来想写的，乏了，这种重任就交给你们了)，可以给一点思路，获取页面，找到所有的歌曲id，搜索就更简单了，构造url 例如薛之谦的搜索界面是这样的 [click me](http://music.taihe.com/search?key=%E8%96%9B%E4%B9%8B%E8%B0%A6),是不是很简单<br><br>So,Just have fun...