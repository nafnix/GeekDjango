# 职位管理系统 - 建模

- 职位名称
- 类别
- 工作地点
- 职位职责
- 职位要求
- 发布人
- 发布日期
- 修改日期

# 创建应用

```sh
python manage.py startapp jobs
```

# 创建Model

## 创建 model 模型

```python
# jobs/models.py

from django.db import models
from django.contrib.auth.models import User
# Create your models here.

JobTypes = [(0, "技术类"), (1, "产品类"), (2, "运营类"), (3, "设计类")]

Cites = [(0, "北京"), (1, "上海"), (2, "深圳")]


class Job(models.Model):
    # blank 指定该字段不可为空
    # choices 指定为一些固定类型
    job_type = models.SmallIntegerField(blank=False, choices=JobTypes, verbose_name="职位类别")
    job_name = models.CharField(max_length=250, blank=False, verbose_name="职位名称")
    job_city = models.SmallIntegerField(choices=Cites, blank=False, verbose_name="工作地点")
    job_reponsibility = models.TextField(max_length=1024, verbose_name="职位职责")
    job_requirement = models.TextField(max_length=1024, blank=False, verbose_name="职位要求")

    # 引用一个另外的模型 Django 内部的 User
    # - null 指定该字段的值可以为空
    # - on_delete 记录删除时对应的数据如何处理。因为 on_delete 使用的是函数，所以末尾不需要加括号
    creator = models.ForeignKey(User, max_length=200, verbose_name="创建人", null=True, on_delete=models.SET_NULL)
    
    created_date = models.DateTimeField(verbose_name="创建日期")
    modified_date = models.DateTimeField(verbose_name="修改时间")
```

## 将 models 引入 admin

![](assets/Pasted%20image%2020220404132948.png)

此时访问管理员页面就可以看到页面中新增了个 `Jobs` 条目：

![](assets/Pasted%20image%2020220404133530.png)

## 数据同步 在数据库中建立 Job 模型

```sh
# 创建脚本
python manage.py makemigrations
# 变动生效
python manage.py migrate 
```

然后就可以访问刚刚新增了条目的页面： http://localhost:2022/admin/jobs/job/

