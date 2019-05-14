title: Python下载大文件
author: Lightfish
tags:
  - Python
categories:
  - Python
date: 2019-01-06 20:39:00
---
# 用Python下载超大文件

>在日常的应用中，我们有时候会用Python下载超大文件，这个时候就需要进行调整了。Just show the code。

<!-- more -->

>代码如下：

```
r = requests.get(url, stream=True)  # 记得设置stream = True
with open(local_filename, 'wb') as f:
    for chunk in r.iter_content(chunk_size=1024): # chunk_size 每次下载1024个字节
        if chunk: # filter out keep-alive new chunks
            f.write(chunk)
            f.flush() # 刷新缓冲区
```

<br><br><br>Just Have Fun...