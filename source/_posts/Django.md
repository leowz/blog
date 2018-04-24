---
title: Django
date: 2017-01-30 18:22:03
tags:
---
`Jan 30 2017`

# 入门
<!--more-->
## 开始一个项目
项目是Django实列的一系列设置集合，包括数据库，setting，和app的设置。
运行`django-admin.py startproject mysite` 会在当前目录下创建一个目录mysite。
这个目录包含4个文件：

```
mysite/
    __init__.py
    mange.py
    settings.py
    urls.py
```
文件作用：
- __init__.py: 让python把该目录当作一组模块的文件。一遍为空不需要改动。
- manage.py: 命令行工具，方便与Django交互，`python mange.py help` 查看具体作用。
- settings.py: 改Django项目的配置文件。
- urls.py: Django的url设置。

这些文件已经构成了一个可运行的Django应用。

## 运行开发服务器

`python manage.py runserver`
默认下本地服务器地址与端口是`http://127.0.0.1:8000/`，也可以改变ip地址或端口运行。
改变端口：
`python manage.py runserver 8080`
改变ip：
`python manage.py runserver 0.0.0.0:8000`
完成这些设置之后，本地网络内其他终端就可以通过访问你的ip地址访问你的服务器了。


# 视图与URL配置

## 第一份视图

```python
from django.http import HttpResponse

def hello(request):
    return HttpResponse(“Hello world”)
```
一个视图就是python的一个函数。这个函数的第一个参数类型是HttpResponse；这个函数返回一个HttpResponse的实列。必须满足这两个条件的函数，才是一个Django可识别的视图。

## URLconf
为了绑定视图函数和URL，我们使用URLconf。其本质是URL正则表达式pattern与此表达式需要调用的视图函数之间的映射表。以这种方式使Django明白哪一个url调用哪一个视图表达式。

## python 搜索路径
搜索路径就是`import` 语句执行时查找系统目录的名单。
可通过以下代码显示路径：
```python
import sys
print sys.path
```
 
通常不必关心搜索路径，python和Django会自动处理。

## Django是怎么处理请求的

当运行`python manage.py runserver`时，脚步将在于mange同一个目录下找setting.py文件。这个文件包涵了有关Django项目的配置信息，如`TEMPLATE_DIRS , DATABASE_NAME `等。最重要的是`ROOT_URLCONF` 它作为URLconf告诉Django哪些python模块将被用到。
自动创建的setting.py包含一个指向自动产生的urls.py的配置。

```python

ROOT_URLCONF = 'mysite.urls'

```

当服务器被访问时，Django根据ROOT_URLCONF的设置装载URLconf。然后按顺序逐个匹配URLconf里的URLpatterns，直到找到一个匹配的。然后调用这个匹配pattern的view函数，并把HttpRequest对象当作第一参数。这个view函数返回一个HttpResponse，Django将其转化成一个带有HTTP头和body的web Response。

总结:
1. 服务器收到访问hello/的请求。 
2. Django 过 ROOT_URLCONF配置来决定根URLconf. 
3. Django在URLconf中的所有URL模式中查找第一个匹配hello/的条目。
4. 如果找到，调用相应的视图函数。  
5. 视图函数返回一个HttpResponse 。
6. Django转换HttpResponse为一个合适的HTTPresponse ，以Webpage显示出来。

# 模版

## 如何使用模版系统
python中使用Django模版的最基本方法如下：
1. 创建一个template对象`template.Template()`;
2. 调用模版对象的render方法，并且传入一套变量context。

## 创建模版对象
Template类在django.template模块中，构造函数接受一个参数，原始模版代码字符串，也可以指定模版文件路径当作参数。
启动python交互解释：
`python manage.py shell`
```python
t = Template('My name is {{ name }}.')
```

创建模版对象之后，可以用context来传递数据给他。一个context是一系列变量和它们值的集合。
context是一个django.template模块里的类。她的构造函数带一个可选参数，一个字典，映射便利和它们的值。调用Template对象的render()方法传递context来填充模版。
```python
t = Template('My name is {{ name }}.')
c = Context({'name': 'Stephane'})
t.render(c) 
```
`t.render(c)`返回一个Unicode字符串对象。

## 模版变量深度访问
在Django模版中访问复杂的数据结构时用点运算符。
```python
person = {'name': 'Sally', 'age': '43'}
t = Template('{{ person.name }} is {{ person.age }} years old.')
c = Context({'person': person})
 t.render(c) 
```
点语法也可以用来调用对象方法。例如，每个python字符串都有upper()方法，可以在模版中使用它们。
```python
t = Template('{{ var }} -- {{ var.upper }} -- {{ var.isdigit }}') 
```
注意这里调用方法时并没有使用圆括号，也无法给改方法传递参数。你只能调用不需参数的方法。

## 基本模版标签
if/else:
```python
{% if today_is_weekend %}
       <p>Welcome to the weekend!</p> 
{% endif %}
```
for:
```python
{% for athlete in athlete_list %}
 <li>{{ athlete.name }}</li> 
{% endfor %}
``` 
ifequal/ifnotequal:
```python
{% ifequal user currentuser %}
       <h1>Welcome!</h1>   
{% endifequal %}
```

## 模版加载
要使用模版加载api，首先必须将模版保存位置告诉Django。
打开settings.py找到TEMPLATE_DIRS。该设置告诉Django模版加载机制去哪里查找模版。添加模版存放目录。
``` python
TEMPLATE_DIRS = ( '/home/django/mysite/templates', )
```
注意目录地址后面逗号不可缺少。

完成设置之后就可以使用模版加载功能了。
```python
from django.template.loader import get_template 
from django.http import HttpResponse 
import datetime 

def current_date(request):
    now = datetime.datetime.now() 
    t = get_template('current_datetime.html') 
    html = t.render(Context({'current_date': now})) 
    return HttpResponse(html)
```
我们使用了`django.template.loader.get_template()`来加载模版而不是手动加载。该函数以模版名称为参数，在文件系统中找出模块的位置，打开并返回一个编译好的Template对象。

## render_to_response()/render()
这是django提供的一条捷径，一次性载入模版渲染并返回。
```python
from django.shortcuts import render_to_response   
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    return render_to_response('current_datetime.html', {'current_date': now})
```
`render_to_response()`的第一个参数必须是使用模版的名称，如果给第二个参数，则是context所使用的字典。

# 模型

## MVC模式
Django遵循MVC模式，是一种MVC框架。以下是MVC在Django中的含义，

- M：数据存取部分，由django数据库层处理。
- V：选择哪些数据展示和怎么展示数据，由视图和模版构成。
- C：根据用户输入委派视图，由Django根据URLconf设置，对给的URL调用相应的函数。

## 数据库配置
要使用数据库首先我们要做初始配置。我们要告诉Django使用什么数据库以及如何连接数据库。
数据库的配置也是在settings.py里，打开这个文件找到：
```python
DATABASE_ENGINE = ‘’ // 使用引擎
DATABASE_NAME = ‘’   // 数据库名字
DATABASE_USER = ‘’     
DATABASE_PASSWORD = '' 
DATABASE_HOST = ‘’     //连接那一台主机的数据库服务器,如果是和Django同一台主机则留空
DATABASE_PORT = ‘’    //端口号
```
我们可以在项目目录下，用`python manage.py shell`来测试。此命令是以正确的配置启用python交互器的一种方法，使python知道加载那个配置文件获取数据库连接信息。
输入以下命令测试数据库是否有配置错误，
```python
  >>> from django.db import connection
   >>> cursor = connection.cursor()
```

## 第一个APP
现在来创建一个包含模型，视图和Django代码的完整应用。
系统对app有一个约定，如果使用了Django的数据库层(模型)，你必须创建一个app。模型必须存放在apps中。
在项目目录下输入以下命令创建app：
`python manage.py startapp books`

这个命令创建了一个books目录生成了一些初始文件。

## 定义模型
Django模型是用python代码形式表述的数据在数据库中的定义。

我们看这一个模型的例子：书籍、作者、出版商
假定下面概念和关系：
- 作者，有姓名、email
- 出版商，有名称、地址、所在城市、省份、国家、及网站
- 书籍，有书名、出版日期。它有一个或多个作者，有一个出版商。

用代码描述上述信息，打开用startapp新建的文件夹，打开models.py文件，加入如下代码：
```python

class Publisher(models.Model):
    name = models.CharField(max_length=30)
    address = models.CharField(max_length=50)     
    city = models.CharField(max_length=60)     
    state_province = models.CharField(max_length=30) 
    country = models.CharField(max_length=50) 
    website = models.URLField() 

class Author(models.Model): 
    first_name = models.CharField(max_length=30) 
    last_name = models.CharField(max_length=40) 
    email = models.EmailField() 

class Book(models.Model): 
    title = models.CharField(max_length=100)    
    authors = models.ManyToManyField(Author) 
    publisher = models.ForeignKey(Publisher)        
    publication_date = models.DateField()
```
首先要注意的是每个数据模型都是`django.db.models.Model`的子类。它们的父类包含了所有必要的和数据库交互的方法，并提供了简洁漂亮的定义数据库字段的语法。
每个模型相当于单个数据库表，每个熟悉就是这个表中的一个字段。属性名就是字段名，她的类型相当于数据库的字段类型。

## 模型安装
完成代码之后，现在在数据库中创建这些表。第一步，在Django项目中激活这些模型，将book添加到配置文件的以安装应用列表中即可完成此步骤。
添加app到settings.py中的INSTALLED_APPS:
```python
INSTALLED_APPS = ( 
 # 'django.contrib.auth’, 
 # 'django.contrib.contenttypes’, 
 # 'django.contrib.sessions’, 
 # 'django.contrib.sites’, 
 'mysite.books', 
)
```
现在可以创建数据库表了，首先用下面命令验证模型的有效性，
`python manage.py validate`

如果模型确认没有问题，通过以下命令可以生成相应的SQL代码
`python manage.py sqlall books`

sqlall命令根据模型生成相应SQL代码，可以把输出的语句复制到数据库客户端执行，或通过管道直接操作。Django也提供更简单的提交SQL语句到数据库的方法：
`python manage.py syncdb`

syndb是同步模型到数据库的一个简单方法。它根据INSTALLED_APPS里设置的app来检查数据库，如果表不存在久创建一个表。syndb不能修改或者删除数据库内容。

## 插入和更新数据  
使用定义的模型生成一个实例对象之后`p = Publisher(…)`，使用`save()`方法把这个对象储存到数据库中，如果数据已经储存过了，则实现对数据更新。

## 选择对象
`Publisher.objects.all()`可以从给的到模型中取出所有记录。

可以通过filer()过滤出特定的表项数据,结果返回一个匹配到的数据列表：
`Publisher.objects.filter(country="U.S.A.", state_province="CA”)`

## 数据排序
`Publisher.objects.order_by("name”)`
使用`order_by()`方法实现根据字段值对返回结果排序。

## 更新多个对象
使用update()方法实现多个对象的更新：
`Publisher.objects.all().update(country='USA')`

## 删除对象
使用delete()方法删除表项：
```python
Publisher.objects.filter(country='USA').delete()
Publisher.objects.all().delete()
```

# 站点管理admin

## django.contrib 包
django.contrib 是一套庞大的功能集，他是django基本代码的组成部分，django框架就是由众多的包含附加组件的基本代码构成的。
管理工具是django.contrib.admin 。

## 激活管理界面
激活Django管理页面需要以下几个步骤：

1. `django.contrib.admin`被加入了setting的INSTALLED_APPS配置中。
2. 保证INSTALLED_APPS中包含了`django.contrib.auth`,`django.contrib.contenttype`,`django.contrib.sessions`这三个需求包。
3. 确保MIDDLEWARE_CLASSES中包含了`django.middleware.common.CommonMiddleware`,`django.contrib.sessions.middleware.SessionMiddleware`,`django.contrib.auth.middleware.AuthenticationMiddleware`。

运行`python manage.py syncdb` 将生成管理界面使用的额外数据库表。
运行`python manage.py createsuperuser`来创建超级用户。

之后在URLconf中配置admin的url。

## 将Models加入到Admin管理中
如果把我们之前建立的models与admin界面连接，我们就可以使用admin的GUI管理我们的数据库了。
在目录下创建一个admin.py文件然后输入代码：

```python

from django.contrib import admin   
from mysite.books.models import Publisher, Author, Book

admin.site.register(Publisher); 
admin.site.register(Author); 
admin.site.register(Book);
```


