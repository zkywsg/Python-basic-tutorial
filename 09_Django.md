## Django

### 组件介绍

- 基本配置文件
- 路由系统
- 模型层：连通数据库
- 模板层：渲染页面
- 视图层
- Cookies和Session
- 分页和发邮件
- 管理后台

### 安装Django

```shell
pip install django==2.2.12
# 检查是否安装完成
pip freeze|grep -i 'Django'
```



### 项目结构

- 创建项目

```shell
django-admin startproject proName
```

- 启动服务

```shell
python manage.py runserver
python manage.py runserver port_num
```

- 结束服务

```shell
# 查询id
sudo lsof -i:8000
kill -9 id 
```

创建后的文件树

- HelloWorld

  - db.sqlite3

  - Manage.py

  - HelloWorld

    - \__init__.py:python包的初始化文件
    - settings.py：项目的配置文件
    - urls.py：项目的路由配置
    - wigs.py：web服务网关的配置文件

    

- Manage.py 子命令入口

```shell
python manage.py
python manage.py runserver
# 创建应用
python manage.py startapp
# 数据库迁移
python manage.py migrate
```

- Settings.py
  - 命名全部需要大写
  - 引入方式 from django.conf import settings
  - BASE_DIR:当前根目录
  - DEBUG：True调试模式
  - ALLOWED_HOST:过滤HOST头，最好配置具体域名。过滤垃圾请求。也可以放局域网内的内网地址。
  - ROOT_URLCONF:主路由配置
  - TEMPLATES：html相关配置
  - DATABASES：配置数据库
  - TIME_ZONE：Asia/Shanghai

### URL和视图函数

URL：统一资源定位符，用来标识互联网上某个资源的地址

- Protocol://hostname[:port]/path\[?query]\[#fragment] 
- 端口默认80
- path路由地址：决定服务器如何处理请求
- query查询：动态网页传递参数
- fragment相当于页面中的锚点



处理URL请求的过程：

- 浏览器输入http://127.0.0.1:8000/page/2003/
- 在配置文件中ROOT URLCONF找到主路由文件。默认情况下在xxx/xxx/urls.py
- Django加载主路由文件中urlpatterns变量
- 依次匹配urlpatterns中的path，匹配到第一个合适的中断
- 匹配成功，调用对应的视图函数，返回响应
- 匹配失败，404

视图函数：用于接受http请求并通过httpresponse对象返回响应的函数。

```python
# 从浏览器接受到http对象
def xxx_view(request,[,xxx]):
    return HttpResponse对象
```

```python
# sample_1  HelloWorld/views.py
from django.http import HttpResponse
def page1_view(request):
    html = "<h1>aaa</h1>"
    return HttpResponse(html)
```

```python
# urls.py
from django.contrib import admin
from django.urls import path
from . import views 
urlpatterns = [
    path('admin/', admin.site.urls),
    path('page/xxx/', views.page1_view)
]
```



### 路由配置

- path转换器：<转换器类型：自定义名称>
  - 作用：如果转换器类型匹配到对应类型的数据，则将数据按照关键字传参的方式传递给视图函数
  - path('page/\<int:page>',views.xxx)
  - 转换器类型int str slug path
- re_path转换器：
  - 在url中可以使用正则表达式进行精准匹配
  - re_path(reg, view,name='xxx')
  - 采用命名分组(?P\<name>Pattern)
    - re_path(r'^(P\<x>\d{1,2})/(?P\<op>\w+)/(?P\<y>\d{1,2})$',views.cal_view)
    - from django.urls import re_path



### 请求和响应

- 请求是浏览器通过http协议发送给服务器端的数据
- 响应是服务器端接收到请求后做出相应的处理后，再回复浏览器端的数据
- Django接收到http请求后，会根据请求数据报文创建HttpResponse对象
- httpresponse对象通过属性描述了请求的所有相关信息



- Django中的请求request的属性
  - path_info:URL字符串
  - Method:http的请求方式
  - GET：QueryDict查询字典的对象，包含get请求方式的数据
  - POST：QueryDict查询字典的对象
  - FILES：类似于字典，包含所有上传信息
  - COOKIES：字典
  - session
  - body
  - get_full_path()
  - META:请求的元数据



- 响应
  - 状态码 HTTP Status Code
  - 常见状态码
    - 200成功
    - 301永久重定向
    - 302临时重定向
    - 404不存在
    - 500内部服务错误
  - 响应对象httpResponse
    - HttpResponse(content=xxx,content_type=text/html,status=200)
    - Content_type
      - Text/html
      - Text/javascript
      - Text/css
      - text/json
    - 子类httpResponseRedirect...



### GET和POST

- 无论什么请求都会在视图函数接受

```python
if request.method == 'GET':
    xxx
elif request.method == 'POST':
    xxx
```

- GET

  - 一般用于向服务器获取数据
  - 场景
    - 在地址栏输入URL
    - 网页超链接
    - form表单中的method是get
  - 获取get参数
    - request.GET['xxx']
    - request.GET.get()
    - request.GET.getlist('xxx')

- POST

  - 一般向服务器提交大量/隐私数据
  - 在form表单中指明路由

  ```html
  <form method='post' action='/login'>
    xxx
  </form>
  ```

  - 服务器接收参数
  - 取消csrf验证，否则POST的时候django会让其403
    - setting.py中的MIDDLEWARE中关于csrf的注释掉



### 设计模式

- MVC架构和MTV架构
  - MVC
    - model：模型层对数据库层进行封装
    - view：用于向用户展示结果，展示什么，怎么展示
    - controller：用于处理请求 获取数据 返回结果
    - 降低耦合度
  - MTV模式
    - model：负责和数据库进行交互
    - template：负责呈现内容到浏览器，怎么呈现数据html
    - view：是核心，负责接收请求，获取数据，返回结果

### 模版配置

- 创建文件夹xxx/templates
- Settings.py中的templates配置项
  - BACKEND：模版引擎
  - DIRS：模板的搜索目录
    - Os.path.join(BASE_DIR,'templates')
  - APP_DIRS：是否要在应用中查找

### 模板加载方式

- loader方法

```python
from django.template import loader
t = loader.get_template('xxx')
html = t.render(字典数据)
return HttpResponse(html)
```

- render直接加载和响应模板

```python
from django.shortcuts import render
return render(request,'模板文件名',dict_data)
```



### 视图和模板的交互

- 视图函数中把字典传递到模板

```python
def xxx_view(request):
    dic = {'xxx':'xx'}
    return render(requset,'xxx.html',dic)
```

- 模板中用{{ 变量名 }}调用视图函数传递过来的参数



### 模板层的标签和变量

- 能传递到模板的数据类型
  - Str/int/list/tuple/dict/func/obj
  - {{ 变量名 }}/{{ 变量名.index }}/{{ 对象.方法 }}/{{ 函数名 }}

```python
def test_html_param(request):
    dic = {}
    dic['int'] = 123
    dic['str'] = 'asd'
    dic['lst'] = ['a','b','c']
    dic['dict'] = {'a':1,'b':2}
    dic['func'] = say_hi
    dic['obj'] = Dog()
    return render(request,'test_html_param.html',dic)
    # 封装局部变量成一个字典  locals
    # return render(request,'xxx.html',locals())
```

- 模板标签

  - {% 标签 %} {% 结束标签 %}

    - {% if %}{% elif %}{% endif %}{% for %}

    ```html
    {% for xx in 可迭代对象 %}
        。。。。
    {% empty %}
        可迭代对象无数据时填充的语句
    {% endfor %}
    ```

  - 内置变量
    - forloop.counter
    - forloop.counter0
    - forloop.recounter
    - forloop.first
    - forloop.last
    - forloop.parentloop

### 模板层的过滤器和继承

过滤器

- 定义：在变量输出时对变量的值进行处理
- {{ 变量｜过滤器1:'xxx'｜过滤器2:'xxx' }}
- 高频使用过滤器
  - lower
  - upper
  - safe：不进行html转义
  - Add:'n'
  - Truncatechars:'n'



继承

- 模板继承可以使父模板的内容重用，子模板直接继承父模板的全部内容，并且可以覆盖父模板中的块
- 父模板中
  - 定义父模板中的块block标签
  - 标识出那些在子模板中是允许被修改的
  - block标签：在父模板中定义，可以在子模板覆盖
- 子模板：
  - 继承模板extends标签
    - {% extends 'xxx.html' %}
    - 重写父模板中的内容块,在父模板中
      - {% block block_name %}
      - {% endblock %}



### URL反向解析

- 绝对地址：http://127.0.0.1:8000/page
- 相对地址：page/1或者/page/1
  - 通常用/page/1



- 反向解析是指在视图或者模板中，用path定义的名称来动态查找相应的路由
  - path(route, views, name='xxx')
  - 根据path的name给url确定了一个唯一的名字，在模板或者视图中，可以通过name来反向推断url信息。
- 模板中
  - {% url 'xxx' %}
  - {% url 'xxx' age='18' %} 其他参数
- 视图中

```python
from django.urls import reverse

print(reverse('xxx', args=[300]))
print(reverse('xxx', kwarys = {}))
>>>获得反向解析出来的地址
```



### 静态文件

- 图片 css js等
- 配置静态文件的访问路径
  - 通过哪个url地址来找静态文件
  - STATIC_UTR = ‘/static/’
- 配置静态文件的存储路径
  - STATICFILES_DIRS = (xxx)
- 模板中通过标签访问静态文件
  - 加载static - {% load static %}
  - {% static '路径' %}

### 应用和分布式路由

- 应用就是单独的业务模块，有自己的路由 视图 模板 模型

```shell
python manage.py start app music
```

- 在setting.py的INSTALLED_APPS进行应用添加



- 分布式路由
  - 主路由可以不处理用户具体的路由，做请求的分发。
  - 主路由中include函数
  - include('app名字.url模块名')

```python
# 主路由
from django.urls import path , include
from . import views

urlpatterns = [
    path('music/',include('music.urls'))
]
```



- 应用下的模板
  - 手动创建templates
  - setting.py开启模板功能
    - APP_DIRS=TRUE
  - 应用的templates和外层的都存在
    - 优先查找外层templates下的模板
    - 然后按INSTALLED_APP顺序查找
  - 解决这种查找方式
    - 可以在app下的templates/app_name 这样注册文件夹

### 模型层

- 负责和数据库进行通信

```python
# 在setting.py的同级__init__里面
import pymysql
pymysql.install_as_MySQLdb()
```

- 创建数据库

  - 进入mysql数据库执行
    - create database 数据库名 default charset=utf8;
    - 通常数据库名和项目名保持一致
  - setting.py进行数据库的配置
    - 修改DATABASES，sqlite3改成mysql

  ```python
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.mysql',
          'NAME': 'helloworld',
          'USER': 'root',
          'PASSWORD': 'xxxxxxxx',
          'HOST': '127.0.0.1',
          'PORT': '3306'
      }
  }
  # 支持mysql/sqlite3/oracle/postgresql
  ```



- 什么是模型
  - 模型是python类，django.db.models.Model派生出的子类
  - 一个模型类代表数据库的一张数据表
  - 模型类中每一个类属性都代表数据库中的一个字段
  - 模型是数据交互的接口，是标识和操作数据库的方法和方式



### ORM框架

- 定义：Object Relational Mapping 即对象关系映射，它是一种程序技术，允许你使用类和对象对数据库进行操作，从而避免通过sql语句操作数据库
- 作用
  - 建立模型类和表之间的对应关系，允许我们通过面向对象的方法来操作数据库
  - 根据设计的模型类生成数据库中的表格
  - 通过简单的配置就可以进行数据库切换
- 优点
  - 只需要面向对象编程，不用面向数据库写代码
    - 对数据库的操作都转化成对类属性和方法的操作
    - 不用编写各种数据库sql语句
  - 实现了数据模型和数据库的解耦，屏蔽了不同数据库操作上的差异
- 缺点
  - 复杂业务使用成本高
  - 映射过程中有性能损失

```python
# 模型类
# bookstore/model.py
from django.db import models
class Book(models.Model):
    title = models.CharField("书名", max_length=50, default='')
    price = models.DecimalField('定价', max_digits=7, decimal_places=2, default=0.0)
```

- 数据库迁移
  - 迁移时Django同步我对模型所做更改到数据库模式的方式
    - 生成迁移文件：python manage.py makemigrations
      - 将应用下的models.py文件生成一个中间文件，并保存在migrations文件夹
    - 执行迁移：python manage.py migrate
      - 把每个应用下的migrations目录中的中间文件同步会数据库



### 模型类的基础操作

- 表结构修改
  - 添加info字段varchar(100)
  - 解决方案
    - 模型类加类属性
    - 执行数据库迁移
  
- 字段类型
  - BooleanField
    - 数据库类型：tinyint(1)
    - 编程中用True/False
    - 数据库中使用1/0
  - CharField
    - 数据库类型：varchar
    - 必须指定max_length参数
  - DateField
    - 数据库类型：date
    - 表示日期
    - 参数 三选一
      - auto_now:每次保存，自动设置字段成当前时间True
      - Auto_now_add：对象第一次被创建时自动设置当前时间True
      - default：设置当前时间
  - DateTimeField
    - 数据库类型：datetime
    - 表示日期时间
    - 参数和DateField相同
  - FloatField
    - double
    - 小数表示
  - DecimalField
    - 数据库类型：decimal
    - 使用小数，和钱相关
    - 参数
      - Max_digits：总的位数
      - Decimal_places:小数点后的数量
  - EmailField
    - 数据库类型：varchar
  - IntegerField
    - 数据库类型：int
    - 编程语言和数据库都用整数
  - ImageField
    - 数据库类型：varchar
    - 作用
      - 储存图片的路径
      - 使用字符串
  - TextField
    - 数据库类型：longtext
    - 作用：表示不定长的字符数据
  
- 字段选项
  - 指定创建的列的额外信息
  - 允许出现多个字段选项
  - primary_key
    - 表示为主键，则数据库不会创建id字段
  - black
    - True，字段可以为空，否则必须填写
  - null
    - True，列的值可以空
  - default
    - 设置所在列的默认值，如果字段null=False建议添加此项
  - db_index
    - 如果true，该列增加索引
  - unique
    - True，字段在数据库中的值必须时唯一
  - Db_column
    - 指定列名
  - Verbose_name
    - 设置字段在admin界面上的名称
  
  ```python
  name = models.CharField(max_length=30, unique=True, null=False, db_index=True)
  ```



### 模型类-Meta类

- 使用Meta类来给模型赋予属性，meta类下有很多内建的类属性，可以对模型类做一些控制。

```python
from django.db import models

class Book(models.Model):
    title = models.CharField("书名", max_length=50, default='')
    price = models.DecimalField('定价', max_digits=7, decimal_places=2, default=0.0)
    info = models.CharField('描述', max_length=100,default='')

    class Meta:
        # 模型类的表名
        db_table = 'book'
        # 给模型对象的一个容易理解的名称，在admin显示
        verbose_name = 'xxx'
        verbose_name_plural = 'xxx'
```



### ORM创建数据

- 数据库中Django_migrations表记录了migrate的全过程，项目各应用中migrate文件应该与之对应，否则migrate会报错
  - 删除迁移文件
  - 删除数据库
  - 重建数据库和迁移文件



- CRUD操作->模型类的管理器对象
- 管理器对象
  - 每一个继承models.Model的模型类，都会有一个objects对象被继承下来

```python
class MyModel(models.Model):
    xxx
MyModel.objects.create()
```



- 创建数据
  - MyModel.objects.create(属性1=xxx)
    - 成功就返回创建好的实体对象
  - 创建实例对象，调用save保存
    - obj.属性=xxx
    - obj.save()

- Django Shell
  - 可以替代view进行直接操作
  - python manage.py shell



### ORM查询数据

- 通过MyModel.objects管理器方法进行查询
  - all
    - MyModel.objects.all()
    - select * from table 
    - 返回QuerySet容器对象，内部存放MyModel实例
  - values
    - MyModel.objects.values()
    - Select 列1，列2 from xxx
    - 返回QuerySet，内部存放字典
      - {列1:xxx, 列2:xxx}
  - value_list
    - MyModel.objects.value_list()
    - 和values区别在返回内部是元组，得用索引去拿了
  - order_by
    - MyModel.objects.order_by('列名','-列名')
      - 默认是升序，降序加-
    - 会用sql的ORDER BY子句进行查询
    - 返回内部是实例
    - Book.objects.values('title').order_by('-price')
      - 内部存放就会变成字典了
  - obj.query
    - 查询原始sql语句
  - filter
    - MyModel.objects.filter()
    - 返回包含此条件的数据集合
    - 返回值
      - QuerySet容器对象，内部存放实例
    - 多个属性是and关系
  - exclude
    - 返回不包含此条件的数据集合
  - get
    - 返回满足条件的一条数据
    - 查询结果大于一条会报错，没有也rasie
- 查询谓词
  - __exact:等值匹配
    - author.objects.filter(id__exact=1)
    - select * from author where id = 1
  - __contains:包含指定值
    - Author.objects.filter(name__contains='w')
    - select * from author where name like '%w%'
  - __startwith
  - __endwith
  - __gt:大于指定值
  - __gte:大于等于
  - __lt:小于
  - __lte：小于等于
  - __in：指定范围
    - Author.objects.filter(country__in=['xxx','xxx])
  - __range
    - author.objects.filter(age__range=(10,20))
    - select * from author between 10 and 20;



### ORM更新操作

- 修改单个实体的某些字段
  - 查：通过get获取实体对象
  - 改：对象.属性修改
  - 保存：save方法
- 批量更新数据
  - 调用Queryset的update(属性=值)

```python
books = Book.objects.filter(id__gt=3)
books.update(price=0)
```



### ORM删除操作

- 单个数据
  - 查找查询结果对应的一个数据对象
  - 调用delete方法删除

```python
try:
    auth = author.objects.get(id=1)
    auth.delete()
except:
    print('fail')
```

- 批量数据
  - 获得QuerySet对象
  - 也用delete()
- 伪删除
  - 通常不会真的吧数据删除
    - 在表中添加布尔字段is_active,默认为true
    - 显示数据的时候，要过滤查询



### F对象和Q对象

- 一个F对象代表数据库中某条记录的字段信息
  - 作用
    - 通常是对数据库中的字段值在不获取的情况下进行操作
    - 用于类属性之间的比较

```python
from django.db.models import F
F('列名')

# 给价格+10
Books.objects.all().update(market_price=F('market_price')+10)

# 不用F
books = Book.objects.all()
for book in books:
    book.market_price = book.market_price+10
    book.save()
```

- 在不用F对象的时候存在一个资源竞争问题 并发问题

```python
def add_like(request, topic_id):
    topic = Topic.objects.get(id=topic_id)
    # 更新 量大时 没有进行锁操作 会导致产生脏数据
    new_like = topic.like+1
    topic.like = new_like
    topic.save()
    # 相当于
    # update topic set like = 1 where id = xxx
    
    # 使用F对象
    topic.like = F('like')+1
    topic.save()
    # 相当于
    # update topic set like = like + 1 where id = xxx
```



- Q对象
  - 当获取查询结果使用复杂的逻辑或/逻辑非等可以使用

```python
from django.db.models import Q
Book.objects.filter(Q(price__lt=20)|Q(pub-'xxx'))
Book.objects.filter(Q(price__lt=20)&Q(pub-'xxx'))
Book.objects.filter(Q(price__lt=20)|~Q(pub-'xxx'))
```



### 聚合查询和直接使用数据库查询

- 聚合查询
  - 针对一个表的某个字段进行统计
  - 整表聚合
    - from django.db.models import *
    - sum，avg，count，max，min
    - MyModel.objects.aggregate(聚合后列名=聚合函数('列名'))
      - 返回字典。变量名：值
  - 分组聚合
    - 基于某个字段分组，对每个组的数据聚合
    - QuerySet.annotate(聚合后列名=聚合函数('列名'))
      - 返回QuerySet
    - 用MyModel.objects.values()进行分组
      - 返回了一组QuerySet
      - 然后进行聚合



- 原生数据库操作
  - 返回RawQuerySet对象

```python
# 用来进行查询的方法raw
books = models.Book.objects.raw('select * from bookstore_book')
for book in book:
    xxxx
```

- 避免sql注入，尽量别用

```python
# 防止注入方式，让django帮忙转义
Book.objects.raw('select * from bookstore_book where id=%s',['1 or 1 = 1'])
```



- 跨过模型类，使用游标链接数据库
  - from django.db import connection

```python
with connection.cursor() as cur:
    cur.execute('sql','params')
```



### Admin管理后台

- 提供了比较完善的后台管理数据库的接口，可以提供开发过程中调用和测试使用

- django会收集已经注册的模型类，为这些模型提供数据库管理界面

- 创建后台管理账号

  - 这个账号是管理后台最高权限账号
  - python manage.py createsuperuser

- 注册自定义模型类

  - 在app的admin.py中导入注册要管理的模型models类

  - from .models import Book

  - Admin.site.register(models类)

  - 后台对象的样式

    - 可以通过修改模型类的\__str__修改

  - 使用模型管理器类改变admin里的对象样式

    - 在<应用app>/admin.py定义模型管理器类

      - class xxxManager(admin.ModelAdmin)

    - 绑定注册模型管理器和模型类

    - ```python
      from django.contrib import admin
      from .models import *
      admin.site.register(model,modelmanager)
      ```

    - ```python
      from django.contrib import admin
      from .models import Book
      
      class BookManager(admin.ModelAdmin):
          list_display = ['id','title','price']
          # 链接到修改页面
          list_display_links = ['title'] 
          # 添加过滤器
          list_filter = ['pub']
          # 添加搜索框
          search_fields = ['title'] 
          # 添加可在列表页编辑的字段
          list_editable = ['price']
      ```

      



### 关系映射

- 一对一映射/一对多映射/多对多映射

- 一对一映射

  ```python
  # on_delete级联删除
  OneToOneField(类名, on_delete=xxx)
  ```

  - 级联删除模式

    - models.CASADE ：模拟SQL约束ON DELETE CASEADE行为，并且删除包含ForeignKey的对象
    - Modes.PROTECT：抛出error阻止被引用对象的删除
    - SET_NULL设置ForeignKey null，需要指定null=True
    - SET_DEFAULT设置ForeignKey为默认值

  - ```python
    # 无外键的模型类
    author1 = Author.objects.create(name='xxx')
    # 有外键的模型类
    # 关联对象
    wife1 = Wife.objects.create(name='xxx',author=author1)
    # 关联对应的主键
    wife1 = Wife.objects.create(name='xxx',author_id=1)
    ```

  - 正向查询：通过外键属性查询

    - ```python
      from .models import Wife
      wife = Wife.objects.get(name='xxx')
      print(wife.author.name)
      ```

  - 反向查询：没有外键的一方，调用反向关联属性(类的小写)

    - ```python
      from .models import Author
      author = Author.objects.get(name='xxx')
      print(author.wife.name)
      ```

- 一对多：一个班级多个学生

  - 一对多要明确角色，在多表上设置外键

  - ```python
    from django.db import models
    
    class Publisher(models.Model):
        name = models.charField('name',max_length=50,unique=True)
    
    class Book(models.Model):
        title = models.charField('bookname',max_length=50)
        publisher = Foreighkey(Publisher, on_delete=models.CASCADE)
    ```

  - 创建数据，先创建一，再创建多

    - ```python
      from .model import *
      
      pub1 = Publisher.objects.create(name='xxx')
      Book.objects.create(title='c++',publisher=pub1)
      Book.objects.create(title='java',publisher_id=1)
      ```

    - 正向查询

      - ```python
        abook = Book.objects.get(id=1)
        print(abook.publisher.name)
        ```

    - 反向查询

      - ```python
        pub1 = Publisher.objects.get(name='xxx')
        # 小写的类名_set
        books = pub1.book_set.all()
        ```

  - 多对多

    - mysql需要第三张表实现，进行解耦

    - ```python
      from django.db import models
      class Author(models.Model):
          name = models.CharField('name',max_length=11)
      class Book(models.Model):
          title = models.CharField('bookname',max_length=11)
          authors = models.ManyToManyField(Author)
      ```

    - 创建数据

      - ```python
        # 先创建author
        author1 = Author.objects.create(name='111')
        author2 = Author.objects.create(name='222')
        book11 = author1.book_set.create(title='python')
        author2.book_set.add(book11)
        
        # 先创建book
        book = Book.objects.create(title='python1')
        author3 = book.authors.create(name='333')
        book.authors.add(author1)
        ```

      - 正向查询

        - ```python
          book.author.all()
          book.authors.filter(age__gt=80)
          ```

      - 反向查询

        - ```python
          author.book_set.all()
          author.book_set.filter()
          ```

          

### Cookies/Session

- 会话

  - 从打开浏览器访问一个网站，到关闭浏览器结束访问，为一次会话
  - HTTP协议是无状态的
  - cookies和session是为了保存状态而产生的存储技术

- Cookies是保存在客户端浏览器上的存储空间

  - 在浏览器上以键值对进行存储-ASCII码

  - 存储数据有生命周期

  - cookie数据是按域存储隔离的

  - cookies的内部数据会每次访问网址的时候携带到服务器端，如果cookies过大会降低响应速度

  - ```python
    # max_age:最大存活描述
    # expires:具体过期时间
    HttpResponse.set_cookie(key, value='', max_age=None, expires=None)
    ```

  - ```python
    # 添加cookie
    responds = HttpResponse('xxx')
    responds.set_cookie('my_val',123,3600)
    return responds
    ```

  - 删除cookies

    - ```python
      HttpResponse.delete_cookie(key)
      ```

  - 获取cookies

    - ```python
      value = request.COOKIES.get()
      ```

- Session把数据存在服务器

  - 使用session需要在浏览器客户端启动cookie，在cookie中储存sessionid

  - 每个客户端都可以在服务器有独立的session

  - session设置

    - INSTALLED_APPS = ['django.contrib.sessions']
    - MIDDLEWARE = ['django.contrib.sessions.middleware.Sessionmiddleware']

  - 使用

    - ```python
      request.session['KEY']
      request.session.get('key','default')
      
      del request.session['KEY']
      ```

    - setting配置

      - SESSION_COOKIE_AGE= 60* 60 *24 * 7 * 2
      - SESSION_EXPORE_ART_BROWSER_CLOSER = TRUE：浏览器关闭就失效

    - django_session是单表设计 ,并且不会删除过期数据

      - python manage.py clearsessions 删除过期数据