# 基于Django Restful Framework 和 Antd Design Pro V4 类似Xadmin 的基于Models 的管理后台前后端生成工具。

![](https://img.shields.io/pypi/v/tyadmin-api-cli)
![](https://img.shields.io/pypi/wheel/tyadmin-api-cli)

更强大，更现代化，自定义度更高的Xadmin！！！
自动生成前后端管理后台，神奇自动对接。免去普通增删改查，筛选，搜索功能开发。

只需要设计好Model，运行两条命令`python manage.py init_admin`,`python manage.py gen_all`

>后端代码生成一个django app到项目目录, 代码归你掌控，无阻二次开发！
>前端生成一个完整的Antd design pro V4项目，代码归你掌控，无阻二次开发！

前端页面， 后端接口，路由，菜单全部自动对接。

已生成示例网站: 

1. https://imooc.funpython.cn/xadmin/index

2. https://mooc.funpython.cn/xadmin

## 内嵌自动dashboard，自动注册现有model count 数据。

![](http://cdn.pic.mtianyan.cn/blog_img/20201010184239.png)

## 全自动的列表展示，搜索，筛选功能。

![](http://cdn.pic.mtianyan.cn/blog_img/20201010192457.png)

## 内嵌富文本支持,仅需把字段定义为`richTextField`,无需任何额外集成。

![](http://cdn.pic.mtianyan.cn/blog_img/20201010192630.png)

## model->前端对应关系

0. ForeignKey自动生成下拉单选菜单, ManyToManyField自动生成下拉多选菜单

>指定`f'{settings.MAIN_DISPLAY}__name'` 关联另一张表哪个字段用于table显示

```python
    course_org = models.ForeignKey(CourseOrg, on_delete=models.CASCADE, verbose_name="所属机构", null=True, blank=True,
                                   help_text=f'{settings.MAIN_DISPLAY}__name')
```

![](http://cdn.pic.mtianyan.cn/blog_img/20201010192310.png)

```
labels = models.ManyToManyField("Label", verbose_name="课程拥有的label", help_text=f'{MAIN_DISPLAY}__title')

```

![](http://cdn.pic.mtianyan.cn/blog_img/20201010194349.png)

![](http://cdn.pic.mtianyan.cn/blog_img/20201010194222.png)

1. richTextField 自动生成富文本

```
    detail = models.richTextField(verbose_name="课程详情", default='')
```

![](http://cdn.pic.mtianyan.cn/blog_img/20201010193352.png)

2. CharField和IntegerField choices 自动生成表单前端下拉选择框。

```python
    GENDER_CHOICES = (
        ("male", "男"),
        ("female", "女")
    )
    gender = models.CharField(max_length=6,verbose_name="性别",choices=GENDER_CHOICES,default="female")
```

![](http://cdn.pic.mtianyan.cn/blog_img/20201010190635.png)

3. ImageField 自动生成带预览的表单上传功能，列表页可选两种展示方式。

```python
    image = models.ImageField(max_length=100,verbose_name="头像", help_text=MAIN_AVATAR)
    image = models.ImageField(verbose_name="封面图",max_length=100, help_text=MAIN_PIC)    
```

![](http://cdn.pic.mtianyan.cn/blog_img/20201010191641.png)

头像样式 `MAIN_AVATAR`:

![](http://cdn.pic.mtianyan.cn/blog_img/20201010191917.png)

图片样式 `MAIN_PIC`:

![](http://cdn.pic.mtianyan.cn/blog_img/20201010191843.png)

4. FileField 字段生成文件上传功能。

```
    download = models.FileField(verbose_name="资源文件", max_length=100)
```

![](http://cdn.pic.mtianyan.cn/blog_img/20201010193041.png)

5. TextField 自动生成前端TextArea 框

```python
 desc = models.TextField(max_length=300, verbose_name="课程描述")
```

![](http://cdn.pic.mtianyan.cn/blog_img/20201010192813.png)

6. BooleanField 自动前端 Boolean 单选

```
    is_banner = models.BooleanField(default=False, verbose_name="是否轮播")
```

![](http://cdn.pic.mtianyan.cn/blog_img/20201010193001.png)

7. IntegerField 自动前端 Int 输入框
```
    learn_times = models.IntegerField(default=0, verbose_name="学习时长(分钟数)")
```
![](http://cdn.pic.mtianyan.cn/blog_img/20201010193445.png)

8. DateField 自动前端 Date选择框

```
birthday = models.DateField(verbose_name="生日")
```
![](http://cdn.pic.mtianyan.cn/blog_img/20201010193614.png)

9. DateTimeField 自动表单 DateTime 选择框。时间范围筛选器。

```
last_login = models.DateTimeField(verbose_name="上次登录")
```

![](http://cdn.pic.mtianyan.cn/blog_img/20201010193852.png)

>注意设置了default，auto_now的不会出现在表单

![](http://cdn.pic.mtianyan.cn/blog_img/20201010195116.png)

## 基于Model定义的表单字段级别自动验证

```
    title = models.CharField(max_length=255, verbose_name="课程标题", unique=True)
```

![](http://cdn.pic.mtianyan.cn/blog_img/20201010194705.png)

## 轻松的后端自定义验证

```
if xxx:
    raise ValidationError({"filed_name": ["错误提示"]})
```

## demo项目快速上手

0. 初始化项目(推荐使用cookiecutter)

```
cookiecutter https://github.com/mtianyan/cookiecutter-tyadmin-demo.git
```
1. 安装tyadmin-api-cli

```
pip install tyadmin-api-cli
```

2. 注册tyadmin-api-cli

```
INSTALLED_APPS = [
    'tyadmin_api_cli',
]
```

3. 初始化 后端app 及 前端项目

```
python manage.py init_admin
```

4. 生成后端自动化的视图，过滤器，路由，序列器。 前端页面及路由菜单。

```
python manage.py gen_all
```

5. 注册生成出的app

```
INSTALLED_APPS = [
    'tyadmin_api_cli',
    'captcha',
    'tyadmin_api'
]
```

6. 注册路由

```
    path('api/xadmin/v1/', include('tyadmin_api.urls')),
```

7. 运行后端项目，运行前端项目

```
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```

```
cd tyadmin
yarn
yarn start
```

访问http://127.0.0.1:8001/xadmin/index