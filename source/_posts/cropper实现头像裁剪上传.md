title: cropper实现头像裁剪上传
author: Light Fish
tags:
  - Django
categories:
  - Django
date: 2019-03-28 20:33:00
---
# Cropperjs+Ajax+Django实现头像裁剪上传

>一直想实现头像的上传，但是我之间实现的只是简单的上传，上传的图片并没有进行处理，这样的话就很难进行二次的头像实现，所以这次就用到了`Cropper`,`cropper`是一款使用简单且功能强大的图片剪裁jQuery插件。So,今天就来简单阐述一下如何实现。关于具体的如何参数说明，可以看这篇博客——[cropper.js 裁剪图片并上传](https://blog.csdn.net/weixin_38023551/article/details/78792400)

<!-- more -->

# Django后台实现

>1.首先当然是后台的路由urls.py配置

```
from django.urls import path
from . import views

app_name='cropper_img'

urlpatterns=[
    path('',views.index_views,name="index_views")
]
```

>2.浏览器也是通过路由然后找到了`views.py`里的视图函数

```
from django.shortcuts import render,HttpResponse
from django.views.decorators.csrf import csrf_exempt
from django.conf import settings
import json

# Create your views here.
@csrf_exempt
def index_views(request):
    if request.method == "GET":
        return render(request, 'cropper_img.html')
    else:
        uploadimg = request.FILES.get('uploadimg','')
        img_name = request.POST.get('img_name','')
        fname = '{}/cropper_img/{}'.format(settings.MEDIA_ROOT, img_name)
        try:
            with open(fname,'wb') as f:
                for c in uploadimg.chunks():
                    f.write(c)
            return HttpResponse(json.dumps({'status':200,'tips':'上传成功'}))
        except:
            return HttpResponse(json.dumps({'status':500,'tips':'上传失败'}))
```

>3.这里就简单实现了后台的管理，但有些细节的我这里就不多说了，比如`settings`里的配置，`MEDIA_ROOT`图片路径的配置，`templates`模板文件的配置，`static`静态文件的配置等等

# JS配置

>因为篇幅有限这里就简单大概说一下流程，后面我会给出github源码。

>页面展示所需文件的引入

```
<link rel="stylesheet" href="{% static 'css/cropper.min.css' %}">
<link rel="stylesheet" href="{% static 'css/ImgCropping.css' %}">
<script src="{% static 'js/jquery-2.1.1.js' %}"></script>
<script src="{% static 'js/cropper.min.js' %}"></script>
<script type="text/javascript" src="{% static 'js/layer.js' %}"></script>
```

>图片选择和展示

```
//图像上传展示
function selectImg(file) {
    if (!file.files || !file.files[0]){
        return;
    }
    var reader = new FileReader();
    reader.onload = function (evt) {
        var replaceSrc = evt.target.result;
        //更换cropper的图片
        $('#tailoringImg').cropper('replace',replaceSrc,false);//默认false，适应高度，不失真
    }
    reader.readAsDataURL(file.files[0]);
}
```

![img](http://qnpic.top/cropper%5C1.jpg)

>图片之所以如上展示也是得益于cropper插件的原因，下面也是cropper的引用

```
    //cropper图片裁剪
    $('#tailoringImg').cropper({
        aspectRatio: 1/1,//默认比例
        preview: '.previewImg',//预览视图
        guides: true,  //裁剪框的虚线(九宫格)
        autoCropArea: 0.5,  //0-1之间的数值，定义自动剪裁区域的大小，默认0.8
        movable: false, //是否允许移动图片
        dragCrop: true,  //是否允许移除当前的剪裁框，并通过拖动来新建一个剪裁框区域
        movable: true,  //是否允许移动剪裁框
        resizable: true,  //是否允许改变裁剪框的大小
        zoomable: true,  //是否允许缩放图片大小
        mouseWheelZoom: true,  //是否允许通过鼠标滚轮来缩放图片
        touchDragZoom: true,  //是否允许通过触摸移动来缩放图片
        rotatable: true,  //是否允许旋转图片
        crop: function(e) {
            // 输出结果数据裁剪图像。
        }
    });
```

>至于如何实现，我这里就不多家阐述了，可以问问[度娘](https://www.baidu.com)。

>裁剪后的处理

```
    //裁剪后的处理
    $("#sureCut").on("click",function () {
        if ($("#tailoringImg").attr("src") == null ){
            return false;
        }else{
            var cas = $('#tailoringImg').cropper('getCroppedCanvas');//获取被裁剪后的canvas
            var base64url = cas.toDataURL('image/png'); //转换为base64地址形式
            //console.log(base64url);
            $("#finalImg").prop("src",base64url);//显示为图片的形式

            //关闭裁剪框
            closeTailor();
        }
    });
    //关闭裁剪框
    function closeTailor() {
        $(".tailoring-container").toggle();
    }
```

![img](http://qnpic.top/cropper%5C2.jpg)

>上传功能的实现

```
    $('#uploadImg').on('click',function(){
        var uploadimg = $('#finalImg').attr("src");//获取base64图片的值
        //console.log(uploadimg);
        if(!uploadimg){
            layer.alert('请选择图片', {
                icon: 0,
                closeBtn: 0,
                skin: 'layer-ext-moon', 
                time: 5000,//该皮肤由layer.seaning.com友情扩展。关于皮肤的扩展规则，去这里查阅
                btn: ['明白了','知道了']
            },function(){
                layer.msg('明白就好',{icon: 1});
            });
        }else{
            bs64 = uploadimg.split(',')[1];//记得去掉前面的标签
            //下面就是如何转换为blob格式的方法
            var binary = atob(bs64);
            var array = [];
            for (var i = 0; i < binary.length; i++) {
                array.push(binary.charCodeAt(i));
            }
            var blob =  new Blob([new Uint8Array(array)], {type: 'image/jpeg'});
            //使用FormData()进行ajax请求，上传图片
            var formdata = new FormData()
            //图片名称
            var img_name = new Date().getTime()+'.jpg';
            formdata.append('uploadimg',blob);
            formdata.append('img_name',img_name);
            $.ajax({
                url: "{% url 'cropper_img:index_views' %}",
                type: 'POST',
                data: formdata,
                dataType: 'json',
                processData: false,
                contentType: false,
                success: function(e){
                    if(e['status']=='200'){
                        layer.msg(e['tips']);
                    }else if(e['status'=='500']){
                        layer.msg(e['tips']);
                    }
                }
            })
        }
        
    })
```

![img](http://qnpic.top/cropper%5C4.jpg)

![img](http://qnpic.top/cropper%5C3.jpg)
>emmm,这次的博客就基本到这里了，简单了介绍了cropper插件配合django的实现，也是做个记录,[源码链接](https://github.com/QGtiger/DjangoProject/tree/master/uploadimg),So

<br><br><br>Just have fun...