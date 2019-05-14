title: wordcloud词云
author: Lightfish
toc: true
tags:
  - Python
  - wordcloud
categories:
  - Python
date: 2018-12-11 22:02:19
---
# WordCloud中英文词云绘制

> 摘要：当我们手上有一篇文档，比如小说、书籍、电影剧本，若想快速了解其主要内容，你这个时候就可以用到WordCloud词云图，显示主要的关键词(配合jieba.analyse更佳)。本博客将介绍常见的词云图绘制，以及Frequency频词云图。

<!-- more -->

##### 首先，我们得准备好我们的文本文件

>这里我就用我服务器上的[链接](http://39.108.219.55:8080/bkcontent?url=https://www.qu.la/book/746/10632452.html)文本,然后爬下来，当做测试文本。

![截图](http://qnpic.top/1.jpg)

- 以下代码实现获取文本
```python
import requests
content = requests.get('http://39.108.219.55:8080/bkcontent?url=https://www.qu.la/book/746/10632452.html').text
```

>获取文本后就是对着文本进行分析，这里我介绍是jieba模块的一个函数，`jieba.analyse.extract_tags()`,其中第一个参数是传进去的文本，`topK`是获取词频的最大词数，`allowPOS`是获取词频的词语类型，详情可查看这片[博客](https://blog.csdn.net/HHTNAN/article/details/77650128),`withWeight`是 是否展示权重，返回的是频词和权重的元组列表。

![img](http://qnpic.top/2.jpg)

![img](http://qnpic.top/3.jpg)

>利用的是`generate_from_text()`方法生成词云，传入的参数是以`空格`为间隔,所以基本代码实现如下：

```python
import jieba.analyse
import matplotlib.pyplot as plt
from wordcloud import WordCloud
import requests


content = requests.get('http://39.108.219.55:8080/bkcontent?url=https://www.qu.la/book/746/10632452.html').text
tags = jieba.analyse.extract_tags(
    content, topK=100, allowPOS=(
        'ns', 'n', 'vn', 'v', 'nr'))
contents = ' '.join(tags)
print(contents)
wc = WordCloud(
    background_color='white',
    font_path=r'C:\Windows\font\kaiu.ttf',
    width=800,
    height=400,
    margin=2,
    max_words=100,
    min_font_size=15,
    random_state=100,
    mode='RGB',
    repeat=False
)
wc.generate_from_text(contents)
plt.imshow(wc)
plt.axis('off')
plt.show()

```

![img](http://qnpic.top/4.jpg)

>通过上面的词云图，你可能会有几个问题：
* 可不可以换背景
* 词云图能不能换成其他的形状
* 有些词汇能不能去掉

>以上这些都是可以更改的，所以，接下来，我就先了解一下WordCloud的API参数及其它 一些方法

|参数|含义|
|------|------|
|font_path|字体的路径，英文不需要设置，中文需要|
|width|宽度，默认400|
|height|高度，默认200|
|margin|边缘，如2|
|ranks_only|...|
|mask|背景图形，如果想根据图片绘制，则需要设置|
|scale|缩放|
|max_words|最多显示词汇|
|min_font_size|最小字号|
|stopwords|停止词的设置|
|random_state|可以理解为词汇的杂乱度|
|background_color|背景颜色，可以16进制|
|colormap|matplotlib 色图，可更改名称进而更改整体风格|
|repeat|默认False|

关于更详细的用法，您可以去官网了解

### 图片背景的词云实现(白底)

* 图片展示
<img src="http://qnpic.top/5.jpg" width=300>

* 代码实现
```
import jieba.analyse
import matplotlib.pyplot as plt
from wordcloud import WordCloud,STOPWORDS,ImageColorGenerator
import requests
import numpy as np
from PIL import Image


content = requests.get('http://39.108.219.55:8080/bkcontent?url=https://www.qu.la/book/746/10632452.html').text
tags = jieba.analyse.extract_tags(
    content, topK=40, allowPOS=(
        'ns', 'n', 'vn', 'v', 'nr'))
contents = ' '.join(tags)
#读取图片
background_image = np.array(Image.open('5.JPG'))
#提取背景图片的颜色
img_color = ImageColorGenerator(background_image)
#设置停止词
stopword = set(STOPWORDS)
wc = WordCloud(
    mask=background_image,
    font_path=r'C:\Windows\font\kaiu.ttf',
    background_color='white',
    width=800,
    height=400,
    margin=2,
    max_words=100,
    min_font_size=15,
    random_state=100,
    repeat=False
)
wc.generate_from_text(contents) # 等价于wc.gernerate(contents)
# #根据图片色设置背景色
# wc.recolor(color_func=img_color)
wc.to_file('yoona.jpg')
#显示图片
plt.imshow(wc)
plt.axis('off')
plt.show()
```
* 截图 ,emm基本实现，可能是图片的质量不行
<img src="http://qnpic.top/yoona.jpg" width=300> 
<br><br><br>Just have fun...