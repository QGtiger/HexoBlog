title: Python Admin开发
author: Lightfish
tags:
  - Python
  - Django
categories:
  - Python
date: 2019-01-04 14:17:00
---

# Django Admin后台管理

>Admin后台系统也称为网站后台管理系统，主要用于对网站前台的信息进行管理，如文字、图片、影音和其他日常使用文件的发布、更新、删除操作，也包括功能信息的统计和管理，如用户信息、订单信息和访客信息等。简单来说，就是对网站数据库和文件等快速操作和管理系统，以使网页内容能够及时得到更新和调整。

<!-- more -->

## 创建超级用户

>`python manage.py createsuperuser`,这就是创建超级用户的代码，用户名和邮箱可以为空，如果用户名为空，默认使用计算机的用户名。输入的密码不会显示在计算机屏幕上。


## 模型展示Admin后台

>要想将Django的App下所定义模型对应的数据库数据展示在Django Admin后台，需要在所在App下的`admin.py`文件添加如下方法：

```
from django.contrib import admin
from .models import *

# Admin后台的title和header
admin.site.site_title=‘Mydjango后台管理’
admin.site.site_header=‘MyDjango’

# 方法一。将模型直接注册岛admin后台
admin.site.register(Product)  # 这里的Product就是models里创建的模型

# 方法二   
# 自定义ProductAdmin类并继承ModelAdmin
# 注册方法一，使用Python装饰器将ProductAdmin和模型Product绑定并注册到后台
@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):
    # 设置显示的字段,列表里都是你创建的模型里的字段
    list_display = [‘id’, ‘name’, ‘weight’, ‘type’]

# 注册方法二
# admin.site.register(Product,ProductAdmin)
```

>日常的开发中都是使用第二种方法。


## Admin的基本设置

### App在admin中的中文显示

>上述的方法将App下的模型注册到了Admin后台。但是在Admin后台显示的模型为英文，这里就可以进一步的设置。首先就是实现App的中文显示。主要有App的`__init__.py`文件实现。实现代码如下(例如这里的App为index)：

```
from django.apps import AppConfig
Import os
# 修改App在admin后台显示的名称
# default_app_config的值来自 apps.py的类名
default_app_config = ‘index.IndexConfig’

# 获取当前App的命名
def get_current_app_name(_file):
    return os.path.split(os.path.dirname(_file_)[-1]
    
# 重写类 IndexConfig
class IndexConfig(AppConfig):
    name = get_current_app_name(__file__)
    verbose_name = ‘ 网站首页 ‘
```

>当项目启动后，程序就会从初识文件__init__获取重写的`IndexConfig`类，类属性 `verbose_name` 用于设置index的中文内容。

### 模型中的字段在admin后台的中文显示

>在models.py中设置类Meta的类属性`verbose_name_plural` 即可实现。显示代码如下

```
# Product模型 设置中文，代码编写在models.py文件中
# 例如下
from django.db import models
class Product(models.Model):
    id = models.AutoField(‘.序号 ’, primary_key=True)
    name = models.CharField(‘.名称 ’, max_length=50)
    weight = models.CharField(‘.重量 ’, max_length=20)
    type = models.ForeighKey(Type, on_delete=models.CASCADE, verbose_name=‘ 产品类型 ’)
    # 设置返回值
    def __str__(self):
        return self.name
    class Meta:
        # 如果只设置verbose_name，在Admin中会显示“产品信息 s”
        verbose_name = ‘ 产品信息 ’
        verbose_name_plural = ‘ 产品信息 ’  
```

## Admin后台的二次优化

>在一个数据中存放了成千上万的数据，这个时候就需要对我们的Django后台进行适当的优化，代码如下(在admin.py文件中进行优化)：

```
# admin.py

from django.contrib import admin
from .models import *

# Admin后台的title和header
admin.site.site_title=‘Mydjango后台管理’
admin.site.site_header=‘MyDjango’

@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):
    # 设置显示的字段,列表里都是你创建的模型里的字段
    list_display = [‘id’, ‘name’, ‘weight’, ‘type’]
    # 设置可搜索的字段并在Admin后台数据生成搜索框，如外键，应使用双下划线连接两个模型的字段
    search_fields = [‘id’, ‘name’, ‘type__type_name’]
    # 设置过滤器，在后台数据的右侧生成导航栏，如有外键，因使用双下划线连接两个模型的字段
    list_filter = [‘name’, ‘type__type_name’]
    # 设置排序方式，[‘id’]为升序，‘-id’为降序
    ordering = [‘id’]
    # 添加新的数据时，设置可添加数据的字段
    fields = [‘name’, ’weight’, ’type’]
    # 设置可读字段，在修改和新增数据时使其无法设置
    readonly_fields = [‘name’]
    
```

>上述代码中，ProductAdmin类分别设置了`list_display`, `search_fields`, `list_filter`, `ordering`, `fields`, `readonly_fields`,每个都有注明方法。