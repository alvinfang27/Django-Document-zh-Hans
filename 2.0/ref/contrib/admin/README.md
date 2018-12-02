> 原文 https://docs.djangoproject.com/en/2.0/ref/contrib/admin/

# Django管理界面	

Django的一个最强大的功能之一就是自动生成管理界面。它读取你的模型（model）中的元数据来生成一个快速的、模型为中心的界面，
授权用户可以使用这个界面管理你的网站内容。建议这个管理界面只作为一个组织内部的管理工具，而不是用于构建整个前端视图。  

管理界面有许多定制的钩子（hooks）,但使用这些钩子要当心。如果你需要提供一个更多的以流程为中心的界面，来操作数据库表和字段，那么可能是时候编写你自己的视图了。

本文我们讨论如何激活、使用和定制Django的管理界面。

## 概述

管理界面在使用`startproject`生成默认项目模板时才可以使用。

作为参考，这里有几个条件：

1. 将`django.contrib.admin`加入你的`INSTALLED_APPS setting`。

2. 管理界面有4个依赖的应用：`django.contrib.auth`、`django.contrib.contenttypes`、`django.contrib.messages`、`django.contrib.sessions`。如果这些应用不在你的`INSTALLED_APPS setting`，把他们加进去。
