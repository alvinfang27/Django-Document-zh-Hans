> 原文 https://docs.djangoproject.com/en/2.0/ref/contrib/admin/

# Django管理站点	

Django的一个最强大的功能之一就是自动生成管理站点。它读取你的模型（model）中的元数据来生成一个快速的、模型为中心的站点，
授权用户可以使用这个站点管理你的网站内容。建议这个管理站点只作为一个组织内部的管理工具，而不是用于构建整个前端视图。  

管理站点有许多定制的钩子（hooks）,但使用这些钩子要当心。如果你需要提供一个更多的以流程为中心的站点，来操作数据库表和字段，那么可能是时候编写你自己的视图了。

本文我们讨论如何激活、使用和定制Django的管理站点。

## 概述

管理站点在使用`startproject`生成默认项目模板时才可以使用。

作为参考，这里有几个条件：

1. 将`django.contrib.admin`加入你的`setting`中的`INSTALLED_APPS`。


2. 管理站点有4个依赖的应用：`django.contrib.auth`、`django.contrib.contenttypes`、`django.contrib.messages`、`django.contrib.sessions`。如果这些不在你的`setting`中的`INSTALLED_APPS`，把他们加进去。

3. 把DjangoTemplates设置为`TEMPLATES`的`backend`，把`django.contrib.auth.context_processors.auth`和`django.contrib.messages.context_processors.messages`设置为其`OPTIONS`。同样，把`django.contrib.auth.middleware.AuthenticationMiddleware`和`django.contrib.messages.middleware.MessageMiddleware`加到`MIDDLEWARE`里。以上这些操作都会在使用`startproject`生成项目时自动生成。所以，如果你是手动填写设置的，就按照上面的做法执行。  

![自动生成的默认Settings文件内容](https://github.com/alvinfang27/Django-Document-zh-Hans/blob/master/2.0/ref/contrib/admin/%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E7%9A%84%E9%BB%98%E8%AE%A4Settings%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9.png)

4. 确定应用程序的哪些模型是可以在管理站点里编辑。

5. 对于每一个模型,可以选择创建一个`ModelAdmin`类，来封装这个模型的定制管理功能和选项。

6. 实例化一个`AdminSite`，告诉它每个模型和`ModelAdmin`类。

7.将`AdminSite`实例加到路由设置（`URLconf`）里。

执行以上步骤之后,您就能够通过访问URL(默认是 `../admin/`)链接到Django管理网站。如果您需要创建一个用户登录,您可以使用[createsuperuser]()命令。

### 其他话题  
[Admin actions]()  
[The Django admin documentation generator]()  
[JavaScript customizations in the admin]()  

> 另请参阅  
> 管理网站需要配置静态文件（图片、Javascript、CSS），请查阅[Serving files]()  
> 遇到问题？请点击[FAQ: The admin]()  

---

## `ModelAdmin` 对象
class ModelAdmin[[source]](https://docs.djangoproject.com/en/2.0/_modules/django/contrib/admin/options/#ModelAdmin)   

ModelAdmin类是在管理站点中表示一个模型。通常,这些存储在你的应用中的一个名为`admin.py`的文件里。让我们看看ModelAdmin的一个非常简单的例子:
```
from django.contrib import admin
from myproject.myapp.models import Author

class AuthorAdmin(admin.ModelAdmin):
    pass
admin.site.register(Author, AuthorAdmin)
```

> 你需要一个`ModelAdmin`对象么？
> 在前面的例子中,ModelAdmin类（还）没有定义任何自定义值。因此,将提供默认管理界面。如果默认的管理界面能满足你的需求,你不需要定义一个ModelAdmin对象。你可以不建立`ModelAdmin`对象，直接注册模型类。以上描述可以简化为:
> ```
> from django.contrib import admin
> from myproject.myapp.models import Author
>
> admin.site.register(Author)
> ```
