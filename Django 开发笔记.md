# 什么是 Django ?

Django 是一款基于 Python 语言的开源 web 框架，采用 MTV 的框架模式，即：

- Model：模型；数据存取层，处理与数据相关的所有事务
- Template：模板；
- Views：视图

# Web 框架原理

理解：所有的 Web 应用本质上是一个 socket 服务端，而浏览器是一个 socket 客户端。

### 自定义 web 框架：

基于 socket 实现一个简单 web

```python
import socket

s = socket.socket()
s.bind(('127.0.0.1', 80))
s.listen()

while True:
    conn, addr = s.accept() #客户端socket对象 客户端地址
    data = conn.recv(8096)  #接收浏览器发来的消息
    print(conn)
    print(addr)
    print(data)
    conn.send(b'OK!') #只能发送字节
    conn.close()
```

访问 `127.0.0.1:80` 后浏览器显示：

![image-20201111111023751](E:\Markdown\images\image-20201111111023751.png)

打印输出：

```python
<socket.socket fd=716, family=AddressFamily.AF_INET, type=SocketKind.SOCK_STREAM, proto=0, laddr=('127.0.0.1', 80), raddr=('127.0.0.1', 13848)>
('127.0.0.1', 13848)
b'GET / HTTP/1.1\r\nHost: 127.0.0.1\r\nConnection: keep-alive\r\nCache-Control: max-age=0\r\nUpgrade-Insecure-Requests: 1\r\nUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36 Edg/86.0.622.63\r\nAccept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9\r\nSec-Fetch-Site: none\r\nSec-Fetch-Mode: navigate\r\nSec-Fetch-User: ?1\r\nSec-Fetch-Dest: document\r\nAccept-Encoding: gzip, deflate, br\r\nAccept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6\r\n\r\n'
```

浏览器发的消息具有一定的格式，即必须符合 HTTP 协议

HTTP，即超文本传输协议（英文：**H**yper**T**ext **T**ransfer **P**rotocol）是一种用于分布式、协作式和超媒体信息系统的应用层协议。HTTP是万维网的数据通信的基础。

 HTTP是一个客户端终端（用户）和服务器端（网站）请求和应答的标准（TCP）

每个 HTTP 请求和响应都应遵循相同的格式，一个 HTTP 包含 Header 和 Body 两部分， 其中 Body 是可选的。HTTP 响应中的 Header  中有一个 `Content-Type` 表命响应的内容格式。如 `text/html` 表示 HTML 网页

HTTP GET 请求的格式

![img](https://images2018.cnblogs.com/blog/867021/201803/867021-20180330221943115-1291906159.png)

HTTP 响应的格式

![img](https://images2018.cnblogs.com/blog/867021/201803/867021-20180330222031912-1851965755.png)

为框架添加 HTTP 协议

```python
import socket

s = socket.socket()
s.bind(('127.0.0.1', 80))
s.listen()


while True:
    conn, addr = s.accept() #客户端socket对象 客户端地址
    data = conn.recv(8096)  #接收浏览器发来的消息
    print(data)
    conn.send(b'HTTP/1.1 200 OK\r\n\r\n')
    conn.send(b'OK!')
    conn.close()
```

### 自定义 web 框架：基于不同路径返回不同的内容

按照 HTTP 协议，客户端发来的消息需要按照一定的格式，通过这些固定格式，可以获取请求方法、url

```python
import socket

s = socket.socket()
s.bind(('127.0.0.1', 80))
s.listen()


while True:
    conn, addr = s.accept()
    data = conn.recv(8096)  # 接收客户端发来消息
    data = str(data, encoding='utf-8')  #将字节转为字符串
    url = data.split('\r\n')[0].split()[1] # 获取路由
    if url == '/index':
        response = b'index'
    elif url == '/home':
        response = b'home'
    else:
        response = b'404 Not Found!'
    conn.send(response)
    conn.close()
```

### 自定义 web 框架：基于不同路径返回不同的内容 - - 函数版

路径不同时调用不同的函数

```python
import socket

s = socket.socket()
s.bind(('127.0.0.1', 80))
s.listen()

def index(url):
    mse = f'这是一个 {url} 页面'
    return bytes(mse, encoding='utf8')

def home(url):
    mse = f'这是一个 {url} 页面'
    return bytes(mse, encoding='utf8')

while True:
    conn, addr = s.accept()
    data = conn.recv(8096)  # 接收客户端发来消息
    data = str(data, encoding='utf-8')  #将字节转为字符串
    conn.send(b'HTTP/1.1 200 OK\r\n')
    url = data.split('\r\n')[0].split()[1]
    if url == '/index':
        response = index(url)
    elif url == '/home':
        response = home(url)
    else:
        response = b'404 Not Found!'
    conn.send(response)
    conn.close()
```

###  自定义 web 框架：基于不同路径返回不同的内容 - - 函数进阶版

如果路由很多，你会进行很多条件判断。为了优化条件判断，建立一个 路由-调用函数对照表

> list1 = [
>     ('/index', index),
>     ('/home', home)
> ]

```python
import socket

s = socket.socket()
s.bind(('127.0.0.1', 80))
s.listen()

def index(url):
    mse = f'这是一个 {url} 页面'
    return bytes(mse, encoding='utf8')

def home(url):
    mse = f'这是一个 {url} 页面'
    return bytes(mse, encoding='utf8')


list1 = [
    ('/index', index),
    ('/home', home)
]

while True:
    conn, addr = s.accept()
    data = conn.recv(8096)  # 接收客户端发来消息
    data = str(data, encoding='utf-8')  #将字节转为字符串
    conn.send(b'HTTP/1.1 200 OK\r\n\r\n')
    url = data.split('\r\n')[0].split()[1]
    func = None
    for i in list1:
        if i[0] == url:
            func = i[1]
    if func:
        response = func(url)
    else:
        response = b'404 not found'
    conn.send(response)
    conn.close()
```

### 自定义 web 框架：返回完整的 HTML 页面

服务器向浏览器发送的是字节流数据，读取html页面，并将其转化为字节流

定义 index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>INDEX</title>
</head>
<body>
这是一个 INDEX DEMO 页面 ！
</body>
</html>
```

定义 home.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HOME</title>
</head>
<body>
这是一个 HOME DEMO 页面！
</body>
</html>
```

读取 HTML 转化为字节

```python
import socket

s = socket.socket()
s.bind(('127.0.0.1', 80))
s.listen()

def index(url):
    # 读取页面并转化为字节
    with open('index.html', 'r', encoding='utf-8') as f:
        msg = f.read()
    return bytes(msg, encoding='utf8')

def home(url):
    with open('home.html', 'r', encoding='utf-8') as f:
        msg = f.read()
    return bytes(msg, encoding='utf8')


list1 = [
    ('/index', index),
    ('/home', home)
]

while True:
    conn, addr = s.accept()
    data = conn.recv(8096)  # 接收客户端发来消息
    data = str(data, encoding='utf-8')  #将字节转为字符串
    conn.send(b'HTTP/1.1 200 OK\r\n\r\n')
    url = data.split('\r\n')[0].split()[1]
    func = None
    for i in list1:
        if i[0] == url:
            func = i[1]
            break
    if func:
        response = func(url)
    else:
        response = b'404 not found'
    conn.send(response)
    conn.close()
```

浏览器访问 `127.0.0.1/index`

![image-20201111140325805](E:\Markdown\images\image-20201111140325805.png)

### 自定义 Web 框架：返回动态的 HTML 页面

上面是一个静态的网页，我们可以通过 **字符串替换** 返回一个静态的网页

index.html 中定义好待替换的字符：例如 ###

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>INDEX</title>
</head>
<body>
这是一个 INDEX DEMO 页面 ！
当前时间为：###
</body>
</html>
```

服务器字符串替换

```python
import socket
import time

s = socket.socket()
s.bind(('127.0.0.1', 80))
s.listen()

def index(url):
    with open('index.html', 'r', encoding='utf-8') as f:
        msg = f.read()
    now = time.time()
    msg = msg.replace('###', str(now))  #字符串替换 ###
    print(msg)
    return bytes(msg, encoding='utf8')

def home(url):
    with open('home.html', 'r', encoding='utf-8') as f:
        msg = f.read()
    return bytes(msg, encoding='utf8')


list1 = [
    ('/index', index),
    ('/home', home)
]

while True:
    conn, addr = s.accept()
    data = conn.recv(8096)  # 接收客户端发来消息
    data = str(data, encoding='utf-8')  #将字节转为字符串
    conn.send(b'HTTP/1.1 200 OK\r\n\r\n')
    url = data.split('\r\n')[0].split()[1]
    func = None
    for i in list1:
        if i[0] == url:
            func = i[1]
            break
    if func:
        response = func(url)
    else:
        response = b'404 not found'
    conn.send(response)
    conn.close()
```

浏览器访问 `127.0.0.1/index`

![image-20201111141145079](E:\Markdown\images\image-20201111141145079.png)

### 自定义 Web 框架：基于 WSGI

对于真实开发的 python web 程序来说，一般会分为两个部分：

- 服务器程序
- 应用程序

服务器程序负责对 socket 服务器进行封装，并在请求到来时，对请求的数据进行整理

应用程序负责具体的逻辑处理，为了方便应用程序的开发，就出现了众多的Web框架，例如：Django、Flask、web.py 等。不同的框架有不同的开发方式，但是无论如何，开发出的应用程序都要和服务器程序配合，才能为用户提供服务。

而服务器程序就需要为不同的框架提供不同的支持，这样混乱的局面无论对于服务器还是框架，都是不好的

这时候，标准化就变得尤为重要。我们可以设立一个标准，只要服务器程序支持这个标准，框架也支持这个标准，那么他们就可以配合使用。一旦标准确定，双方各自实现。这样，服务器可以支持更多支持标准的框架，框架也可以使用更多支持标准的服务器。

WSGI（Web Server Gateway Interface）就是一种规范，它定义了使用Python编写的web应用程序与web服务器程序之间的接口格式，实现web应用程序与web服务器程序间的解耦

常用的WSGI服务器有uwsgi、Gunicorn。而Python标准库提供的独立WSGI服务器叫wsgiref，Django开发环境用的就是这个模块来做服务器。

利用 wsgiref 替换 web 框架的 socket server 部分

```python
import time
from wsgiref.simple_server import make_server


def index(url):
    with open('index.html', 'r', encoding='utf-8') as f:
        msg = f.read()
    now = time.time()
    msg = msg.replace('###', str(now))
    return bytes(msg, encoding='utf8')

def home(url):
    with open('home.html', 'r', encoding='utf-8') as f:
        msg = f.read()
    return bytes(msg, encoding='utf8')


list1 = [
    ('/index', index),
    ('/home', home)
]

def run_server(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html;charset=utf8'), ])  #设置HTTP响应的状态码及头信息
    url = environ['PATH_INFO']  #取到用户输入的url
    func = None
    for i in list1:
        if i[0] == url:
            func = i[1]
            break
    if func:
        response = func(url)
    else:
        response = b'404 not found'
    return [response, ]

if __name__ == '__main__':
    httpd = make_server('127.0.0.1', 80, run_server)
    httpd.serve_forever()
```

### 自定义 Web 框架：利用 jinjia2 进行模板数据渲染

利用现场的模板渲染工具：

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>INDEX</title>
</head>
<body>
这是一个 INDEX DEMO 页面 ！

<h1>姓名：{{ name }}</h1>
<h1>爱好：</h1>
<ul>
    {% for hobby in hobby_list %}
    <li>{{ hobby }} </li>
    {% endfor %}
</ul>
</body>
</html>
```

服务端：

```python
from wsgiref.simple_server import make_server
from jinja2 import Template

def index():
    with open('index.html', 'r', encoding='utf-8') as f:
        data = f.read()
    # 利用 jiaji2 模板渲染
    template = Template(data)
    ret = template.render({'name': "Alex", "hobby_list": ["烫头", "泡吧"]})
    return [bytes(ret, encoding='utf8'), ]

def home():
    with open('home.html', 'rb') as f:
        data = f.read()
    return [data, ]


list1 = [
    ('/index', index),
    ('/home', home)
]

def run_server(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html;charset=utf8'), ])  #设置HTTP响应的状态码及头信息
    url = environ['PATH_INFO']  #取到用户输入的url
    func = None
    print(url)
    for i in list1:
        if i[0] == url:
            func = i[1]
            break
    if func:
        return func()
    else:
        return [bytes("404没有该页面", encoding="utf8"), ]


if __name__ == '__main__':
    httpd = make_server('127.0.0.1', 80, run_server)
    httpd.serve_forever()
```

浏览器访问 127.0.0.1/index

![image-20201111164620920](E:\Markdown\images\image-20201111164620920.png)



# 安装 Django

> pip install django

# 创建 Django 项目

## 1、利用 Pycharm 创建 Django 项目

1、菜单栏 - File - New Project

![image-20200710171341770](E:\Markdown\images\image-20200710171341770.png)

2、

![image-20200710172836794](E:\Markdown\images\image-20200710172836794.png)

## 2、利用 CMD 创建 Django 项目

#### 1、创建虚拟环境

命令行创建：

> python -m venv env

激活：

> cd env/Scripts

> activate.bat

激活后：

![image-20201112142003253](E:\Markdown\images\image-20201112142003253.png)

虚拟环境安装 django

> pip install django

#### 2、创建 Django 项目

虚拟环境激活情况下：

> cd e:

> django-admin startproject MyDjango

创建 app

> cd MyDjango

> python manage.py startapp index

创建后 文件目录

![image-20201112144040060](E:\Markdown\images\image-20201112144040060.png)

启动项目：

cmd 命令行，在项目目录下运行：

> python manage.py runserver 80

![image-20201112144514054](E:\Markdown\images\image-20201112144514054.png)

浏览器访问：`127.0.0.1:80`

![image-20201112144547962](E:\Markdown\images\image-20201112144547962.png)

# Django 配置信息

项目配置是根据实际开发需求从而对整个Web框架编写相关配置信息。配置信息主要由项目的settings.py实现，主要配置有项目路径、密钥配置、域名访问权限、App列表、配置静态资源、配置模板文件、数据库配置、中间件和缓存配置

### 基本配置信息

一个简单的项目必须具备的基本配置信息有：项目路径、密钥配置、域名访问权限、App列表和中间件

```python
import os
from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent  # 项目路径


# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = ')mm8s71l7a!budx)5^j+61%css416=sg_!flz^tmh-pu)9c(39'  # 密钥配置

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True  # 调试模式

ALLOWED_HOSTS = []  # 域名访问权限


# Application definition
# App 列表
INSTALLED_APPS = [
    'django.contrib.admin',  # 内置后台管理系统
    'django.contrib.auth',  # 内置的用户认证系统
    'django.contrib.contenttypes',  # 记录项目中所有的 model 元数据（Django的ORM框架）
    'django.contrib.sessions',  # session 会话功能
    'django.contrib.messages',  # 消息提示功能
    'django.contrib.staticfiles',  #查找静态资源路径
    'index',  # app
]

```

- BASE_DIR：项目路径，当前项目在系统的具体目录
- SECRET_KET：项目创建自动生成的随机值，用于用户密码、CSRF机制、会话 Sission 数据加密
- DEBUG：调试模式 。开发调试阶段应设为 True，开发调试过程中会自动检测代码是否发生更改，并根据检测结果执行是否刷新重启系统；项目上线时应设为 False
- ALLOWED_LISTS：域名访问权限，设置可访问的域名。DEBUG为True并且ALLOWED_HOSTS为空时，项目只允许以localhost或127.0.0.1在浏览器上访问。当DEBUG为False时，ALLOWED_HOSTS为必填项，否则程序无法启动，如果想允许所有域名访问，可设置ALLOW_HOSTS=['*']。
- INSTALLED_APPS：告诉 Django 有哪些 App。项目创建了 App，须在其中添加 App 名称

### 静态资源配置

静态资源指的是网站中不会改变的文件。在一般的应用程序中，静态资源包括CSS文件、JavaScript文件以及图片等资源文件

静态文件的存放主要由配置文件settings.py设置

```python
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/3.1/howto/static-files/

STATIC_URL = '/static/'  # 静态资源访问路由

# 静态文件目录
# STATICFILES_DIRS = [
#     os.path.join(BASE_DIR, 'static'),  # 根目录下的静态文件目录
#     os.path.join(BASE_DIR, 'index/static'),  # app 下的静态文件目录
# ]

# 静态资源根目录，部署时收集整个项目的静态资源，构建静态资源与服务器的映射
# STATIC_ROOT = os.path.join(BASE_DIR, 'all_static')
```

当项目启动时，Django 会根据静态资源存放路径去查找相关的资源文件，查找功能主要由 App 列表 INSTALLED_APPS 的 staticfiles 实现。

这里需注意：

- STATIC_URL：静态资源起始 url，该属性必须配置且不能为空。浏览器访问静态资源时，静态资源的上级目录必须为该属性值。如果没有配置  STATICFILES_DIRS ，STATIC_URL 只能识别 App 里的 static 静态资源，且静态资源文件命名必须为 `static`
- STATICFILES_DIRS：静态资源目录，可选配置。属性值为列表或元组形式，每个元素代表一个静态资源文件夹，这些文件夹可自行命名
- 浏览器访问项目静态资源时，无论静态资源文件如何命名，访问时静态资源的上级目录必须为 STATIC_URL 属性值
- 静态资源的查找顺序为：先按 STATICFILES_DIRS 配置顺序查找，找不到再按照 INSTALLED_APPS 配置顺序查找

### 模板路径配置

在Web开发中，模板是一种较为特殊的HTML文档。这个HTML文档嵌入了一些能够让Python识别的变量和指令，然后程序解析这些变量和指令，生成完整的HTML网页并返回给用户浏览。模板是Django里面的MTV框架模式的T部分，配置模板路径是告诉Django在解析模板时，如何找到模板所在的位置

```python
TEMPLATES = [
    {
         # 模板引擎，内置的模板引擎有DjangoTemplates和jinja2.Jinja2
        'BACKEND': 'django.template.backends.django.DjangoTemplates', 
        # 设置模板所在路径
        'DIRS': [
            BASE_DIR / 'templates',  # 根目录，用于存放各 app 共用的模板文件
            BASE_DIR / 'index/templates',  # app 模板目录，存放app需要使用的模板文件
                 ],  

        'APP_DIRS': True,  # 是否在app中查找模板文件
        # 用于填充在 RequestContext 中上下文的调用函数，一般情况下不做任何修改
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

```

在项目 根目录 及 App 根目录 创建 templates 模板

根目录 templates 通常存放共用的模板文件，能够供各个 App 的模板文件调用，实现代码的重用

App 目录下的 templates 是存放 当前 app 所需要使用的模板文件



### 数据库配置

数据库配置时选择项目所使用的数据库的类型，不同的数据库需要设置不同的数据库引擎，数据库引擎用于实现项目与数据库的连接，Django 提供四种数据库引擎

- django.db.backends.sqlite3
- django.db.backends.mysql
- django.db.backends.postgresql
- django.db.backends.oracle

项目创建时默认使用 sqlite3 数据库

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    },
}
```

使用 mysql 数据库时需安装 mysqlclient 模块

mysqlclient 模块需大于一定的版本，如果发现 mysqlclient 版本大于 Django 版本要求但仍提示 mysqlclient 版本过低，可修改 `django.db.backends.mysql.base`文件中的 

```python
version = Database.version_info
if version < (1, 4, 0):
    raise ImproperlyConfigured('mysqlclient 1.4.0 or newer is required; you have %s.' % Database.__version__)

```

Mysql 数据库配置信息：

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',  # 数据库引擎
        'NAME': 'django_db',  # 数据库名
        'USER': 'root',  # 用户
        'PASSWORD': '1234',  # 密码
        'HOST': '127.0.0.1',  # 数据库地址
        'PORT': '3306',  # 数据库端口
    },
}
```

如果 Mysql 版本大于 5.7，Django 连接 Mysql 数据库时会提示 `django.db.utils.OperationalError` 的错误信息，这是由于 Mysql 8.0 密码加密方式发生了改变，8.0 版本用户密码采用的时 cha2 加密方法。

为解决这个问题可通过 sql 语句将 8.0 版本的加密方法改回源来的加密方式

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH msyql_native_password BY 'newpassword';
FLUSH PRIVILEGES;
```

如果项目需要配置多个数据库：

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',  # Django 提供4种数据库引擎
        # 'ENGINE': 'django.db.backends.mysql',
        # 'ENGINE': 'django.db.backends.postgresql',
        # 'ENGINE': 'django.db.backends.oracle',
        'NAME': BASE_DIR / 'db.sqlite3',
    },
    # 设置多个数据库，添加多个字典键值对
    'MyDjango': {
        'ENGINE': 'django.db.backends.mysql',  # 数据库引擎
        'NAME': 'django_db',  # 数据库名
        'USER': 'root',  # 用户
        'PASSWORD': '1234',  # 密码
        'HOST': '127.0.0.1',  # 数据库地址
        'PORT': '3306',  # 数据库端口
    },
}
```



### 中间件配置

中间件（Middleware）是处理 Django 的 请求（request）和 响应（response）对象的钩子

从请求到响应的过程中，当Django接收到用户请求时，Django 首先经过中间件处理请求信息，执行相关的处理，然后将处理结果返回给用户，中间件执行流程如图

![image-20201112180248624](E:\Markdown\images\image-20201112180248624.png)

开发者可根据自己的开发需求自定义中间件，只要将自定义的中间件添加到配置属性 MIDDLEWARE 中即可激活

```python
# 中间件
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',  # 内置安全机制，保护用户与网站的通信安全
    'django.contrib.sessions.middleware.SessionMiddleware',  # 会话 Session 功能
    'django.middleware.locale.LocaleMiddleware',  # 使用中文
    'django.middleware.common.CommonMiddleware',  # 处理请求信息，规范化请求内容
    'django.middleware.csrf.CsrfViewMiddleware',  # 开启 CSRF 防护功能
    'django.contrib.auth.middleware.AuthenticationMiddleware',  # 开启内置的用户认证系统
    'django.contrib.messages.middleware.MessageMiddleware',  # 开启内置的信息提示功能
    'django.middleware.clickjacking.XFrameOptionsMiddleware',  # 防止恶意程序点击挟持
]
```

配置属性MIDDLEWARE的数据格式为列表类型，每个中间件的设置顺序是固定的，如果随意变更中间件很容易导致程序异常

# 编写 URL 规则

URL（Uniform Resource Locator，统一资源定位符）是对可以从互联网上得到的资源位置和访问方法的一种简洁的表示，是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的URL，用于指出文件的路径位置。简单地说，URL就是常说的网址，每个地址代表不同的网页，在Django中，URL也称为URLconf。

### URL 编写规则

为符合开发规范，每个 App 中都应有一个路由配置文件，即 url.py

所有属于 App 的 url 都应写到对应 App 的路由文件 url.py 中

根目录下的路由文件 url.py 统一管理所有 app 的路由文件。

根目录路由配置

```python
from django.contrib import admin  # 导入admin 功能模块
from django.urls import path, include  # 导入url编写模块

urlpatterns = [
    path('admin/', admin.site.urls),  # 设定admin的URL
    path('', include('index.urls')),  # 将域名路由分发给 index.urls 处理
    path('user/', include('user.urls')),  #将路由 user/ 分发给 user.ursl 处理
]
```

App 路由配置

```python
from django.urls import path,
from . import view   # 导入视图处理模块

urlpatterns = [
    path('', view.index)  # 表示该路由交给 views.index 函数处理
]
```

视图函数 views 中定义 index 函数

```python
from django.shortcuts import render, HttpResponse

# Create your views here.

def index(request, *args, **kwargs):
    return HttpResponse('Hello world ! This Is A Index Page !')
```

浏览器访问 127.0.0.1:8000

![image-20201113174125913](E:\Markdown\images\image-20201113174125913.png)



### 带变量的 URL

在日常开发中，有时候一个 URL 可以代表多个不同的页面，如编写带有日期的 URL，开发者编写365个不同的URL才能实现，这种做法明显是不可取的。因此，Django在编写URL时，可以对URL设置变量值，使URL具有多样性。

URL 变量类型：

- 字符类型：匹配任何非空字符串，但不包括斜杠；未指定类型时，默认为该类型
- 整型：匹配 0 和正整数
- slug：可理解为注释、后缀或附属等概念，常作为URL的解释性字符。可匹配任何ASCII字符以及连接符和下画线，能使URL更加清晰易懂。比如网页的标题是“13岁的孩子”，其URL地址可以设置为“13-sui-de-hai-zi”。
- uuid：匹配一个uuid格式的对象。为了防止冲突，规定必须使用破折号并且所有字母必须小写，例如075194d3-6885-417e-a8a8-6c931e272f00。

在 URL 中使用 `<>` 添加变量，其中以 `:` 区分类型变量类型和变量名，`:` 前表示变量类型，`:`后表示变量名

在 index.urls 中添加变量

```python
from django.urls import path, re_path
from . import views

urlpatterns = [
    path('', views.index),  # 表示该路由交给 views.index 函数处理
    path('<year>/<int:month>/<slug:day>', views.mydate),  # 设置变量
]
```

views.mydate

```python
from django.shortcuts import render, HttpResponse

# Create your views here.

def mydate(request, year, month, day):
    return HttpResponse(str(year) + '/' + str(month) + '/' + str(day))
```

浏览器访问

![image-20201113175238465](E:\Markdown\images\image-20201113175238465.png)

### URL 中引入 正则表达式

Django 中 re_path 模块 可以向 URL 中引入正则表达式，正则表达式可以对 URL 进行截取和判断

路由中 配置

```python
from django.urls import path, re_path
from . import views

urlpatterns = [
    path('', views.index),  # 表示该路由交给 views.index 函数处理
    path('<year>/<int:month>/<slug:day>', views.mydate),
    re_path('(?P<year>[0-9]{4})/(?P<month>[0-9]{2}])/(?P<day>[0-9]{2})/', views.mydate),
```



### 设置参数 name

在开发过程中，如果 URL 有变动，我们需要逐个修改 HTML 页面中的 url。如果页面很多，我们将会做大量的工作。参数name 可以解决这个问题。

app 路由：

```python 
from django.urls import path, re_path
from . import views

urlpatterns = [
    path('', views.index),  # 表示该路由交给 views.index 函数处理
    path('<year>/', views.mydate), 
]
```































开发模式

- 普通开发模式（前后端放一起写）
- 前后端分离

# 后端开发

- API 接口开发（返回 Httpresponse）

# FBV/CBV

## FBV：function base view

视图中定义视图函数

```python
def users(request):
    user_list = ['alex','oldboy']
    return HttpResponse(json.dumps(user_list))
```

路由系统中

```python
urlpatterns = [
    path('users/', views.users),
]
```



## CBV：class base view

视图类需要继承 `django.views` 中的 `View` 方法

```python
from django.views import View

class StudentsView(View):

    def get(self, request, *args, **kwargs):
        return HttpResponse('GET')

    def post(self, request, *args, **kwargs):
        return HttpResponse('POST')

    def put(self, request, *args, **kwargs):
        return HttpResponse('POST')

    def delete(self, request, *args, **kwargs):
        return HttpResponse('POST')
```

路由系统中需调用视图类的 `as_view()` 方法

```python
urlpatterns = [
    path('students/',views.StudentsView.as_view())
]
```

基于反射实现根据请求方式不同，执行不同的方法

原理：

​	`url` -> `view` -> `dispatch(反射执行其他：GET/POST/DELETE/PUT等等)` 

```python
class StudentsView(View):

    def dispatch(self, request, *args, **kwargs):
        fuct = getattr(self, request.method.lower())
        return fuct(request,*args, **kwargs)

    def get(self, request, *args, **kwargs):
        return HttpResponse('GET')

    def post(self, request, *args, **kwargs):
        return HttpResponse('POST')

    def put(self, request, *args, **kwargs):
        return HttpResponse('POST')

    def delete(self, request, *args, **kwargs):
        return HttpResponse('POST')
```

继承（多个类公用的功能，为了避免重复编写）

```python
class MyBaseView(object):
    def dispatch(self, request, *args, **kwargs):
        print('before')
        ret = super(MyBaseView,self).dispatch(request, *args, **kwargs)
        print('after')
        return ret

class StudentsView(MyBaseView, View):

    def get(self, request, *args, **kwargs):
        print('GET方法')
        return HttpResponse('GET')

    def post(self, request, *args, **kwargs):
        return HttpResponse('POST')

    def put(self, request, *args, **kwargs):
        return HttpResponse('POST')

    def delete(self, request, *args, **kwargs):
        return HttpResponse('POST')
```



# CSRF

### 1、Django中间件

- process_request
- process_view
- process_response
- process_exception
- process_render_template

### 2、中间件作用

- 权限
- 用户登录
- csrf
  - process_view方法
    - 检查视图是否被 `@csrf_exempt`（免除 csrf 认证）
    - 去请求体或 cookie 中获取 token

### 3、CSRF配置

#### 全局配置

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware', #全局使用 csrf 认证
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

#### FBV局部配置（装饰器）

- `@csrf_exempt` 不使用 csrf 认证
- `@csrf_protect` 使用 csrf 认证

```python
from django.views.decorators.csrf import csrf_exempt

@csrf_exempt
def users(request):
    user_list = ['alex','oldboy']
    return HttpResponse(json.dumps(user_list))
```

#### CBV局部配置（装饰器）

- 装饰器需加在 `dispatch` 方法，单独方法无效 ：`@method_decorator(csrf_exempt)`
- 装饰器加在类上，指定适用方法：`@method_decorator(csrf_exempt, name='dispatch')`

方法一：

```python
from django.utils.decorators import method_decorator

class StudentsView(MyBaseView, View):
    
    @method_decorator(csrf_exempt)
    def dispatch(self, request, *args, **kwargs):
        return super(MyBaseView,self).dispatch(request, *args, **kwargs)
    
    def get(self, request, *args, **kwargs):
        print('GET方法')
        return HttpResponse('GET')

    def post(self, request, *args, **kwargs):
        return HttpResponse('POST')

    def put(self, request, *args, **kwargs):
        return HttpResponse('POST')

    def delete(self, request, *args, **kwargs):
        return HttpResponse('POST')
```

方法二

```python
from django.utils.decorators import method_decorator

@method_decorator(csrf_exempt, name='dispatch')
class StudentsView(MyBaseView, View):
    
    def get(self, request, *args, **kwargs):
        print('GET方法')
        return HttpResponse('GET')

    def post(self, request, *args, **kwargs):
        return HttpResponse('POST')

    def put(self, request, *args, **kwargs):
        return HttpResponse('POST')

    def delete(self, request, *args, **kwargs):
        return HttpResponse('POST')
```



# Restful 规范（建议）

### 1、接口开发

路由

```python
urlpatterns = [
    path('get_order/', views.get_order),
    path('add_order/', views.add_order),
    path('update_order/', views.update_order),
    path('del_order/', views.del_order),
]
```

视图

```python
def get_order(request):
    return HttpResponse('获取')
def add_order(request):
    return HttpResponse('添加')
def update_order(request):
    return HttpResponse('更新')
def del_order(request):
    return HttpResponse('删除')
```

### 2、Restful 规范（建议）

根据 method 不同做不同的操作

#### 基于 FBV 

路由

```python
urlpatterns = [
    path('order/', views.order),
]
```

视图

```python
def order(requets):
    if requets.method =='GET':
        return HttpResponse('获取订单')
    elif requets.method == 'POST':
        return HttpResponse('创建订单')
    elif requets.method == 'PUT':
        return HttpResponse('更新订单')
    elif requets.method == 'DELETE':
        return HttpResponse('删除订单')
```

#### 基于 CBV 

路由

```python
urlpatterns = [
    path('order/', views.OrderView.as_view()),
]

```

视图

```python
class OrderView(View):
    def get(self, request, *args, **kwargs):
        return HttpResponse('获取订单')
    def post(self, request, *args, **kwargs):
        return HttpResponse('创建订单')
    def put(self, request, *args, **kwargs):
        return HttpResponse('更新订单')
    def delete(self, request, *args, **kwargs):
        return HttpResponse('删除订单')
```



# 虚拟环境 Virtualenv

在开发过程中，为了解决不同模块对不同 Python 版本的依赖，我们可以创建一个独立（隔离）的 Python 环境，也就是虚拟环境。 目前有两种用于创建 Python 虚拟环境的常用工具

- **`venv`** 在 Python 3.3 和更高版本中默认可用，并在 Python 3.4 和更高版本将 `pip` 和 `setuptools` 安装到创建的虚拟环境中。 
- **`virtualenv`** 需要单独安装(`pip`)，但支持 Python 2.7+ 和 Python 3.3+，并且 `pip`、`setuptools` 和 `wheel` 默认情况下始终安装到创建的虚拟环境中（无论 Python 版本如何）。

### 使用 `venv`

**创建**：`python -m venv <dir>`

```python
python -m venv <dir>
```

**激活**：`source <dir>/bin/activat e` 或 `<dir>\Scripts\activate.bat`

```python
#linux
source <dir>/bin/activate
#windows
<dir>\Scripts\activate.bat
```

**退出**：`deactivate`

### 使用 `virtualenv`

**安装模块**：`pip install virtualenv`

```python
pip install virtualenv
```

**创建**：在指定文件夹创建一个隔离的 `virtualenv` 

```python
virtualenv myproject
```

**激活(启动)**：激活隔离的环境（`virtualenv`）

```python
#linux
source myproject/bin/activate
#windows
myproject\Scripts\activate.bat
```

默认情况下，`virtualenv` 不会使用系统全局模块，通过 `--system-site-packages` 参数可创建使用系统全局模块的 `virtualenv`。或者利用 `--no-site-packages` 创建不包含任何第三方包的纯净`virtualenv`

**虚拟环境相关命令**
`mkvirtualenv envname` 默认创建目录为user目录下（可通过修改 Python 安装目录下的 Scripts 目录中的 `mkvirtualenv.bat` 文件中24行：set "venvwrapper.default_workon_home=指定目录"，并将目录添加到环境变量）

`lsvirtualenv -b` 查看虚拟环境

`workon envname` 进入虚拟环境 envname

`deactivate` 退出虚拟环境

`rmvirtualenv envname` 删除虚拟环境

# 创建 Django 项目

### CMD 创建

启动虚拟环境：

安装Django： pip install django  指定版本 pip3 install django==2.0

新建项目： django-admin.py startproject mysite

新建APP : python manage.py startapp blog

启动：python manage.py runserver 8080

# REST_framework

- REST与技术无关，代表的是一种软件架构风格，REST是Representational State Transfer的简称，中文翻译为“表征状态转移”
- REST从资源的角度类审视整个网络，它将分布在网络中某个节点的资源通过URL进行标识，客户端应用通过URL来获取资源的表征，获得这些表征致使这些应用转变状态
- 所有的数据，不过是通过网络获取的还是操作（增删改查）的数据，都是资源，将一切数据视为资源是REST区别与其他架构风格的最本质属性
- 对于REST这种面向资源的架构风格，有人提出一种全新的结构理念，即：面向资源架构（ROA：Resource Oriented Architecture）

## RESTful API设计规范

- 协议：

  - API与用户的通信协议，总是使用 [HTTPS协议](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)。

- 域名：

  - https://api.example.com             尽量将API部署在专用域名（会存在跨域问题）
  - https://example.org/api/            API很简单

- 版本：

  - URL，如：https://api.example.com/v1/
  - 请求头                         跨域时，引发发送多次请求

- 路径：视网络上任何东西都是资源，均使用名词表示（可复数，名词与数据库字段对应）

  - https://api.example.com/v1/zoos
  - https://api.example.com/v1/animals
  - https://api.example.com/v1/employees

- method

  - GET   ：从服务器取出资源（一项或多项）
  - POST  ：在服务器新建一个资源
  - PUT   ：在服务器更新资源（客户端提供改变后的完整资源）
  - PATCH ：在服务器更新资源（客户端提供改变的属性）
  - DELETE ：从服务器删除资源
  
- 过滤，通过在url上传参的形式传递搜索条件

  - ?limit=10：指定返回记录的数量
  - ?offset=10：指定返回记录的开始位置
  - ?page=2&per_page=100：指定第几页，以及每页的记录数
  - ?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序
  - ?animal_type_id=1：指定筛选条件

- 状态码

  - 200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）
  - 201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
  - 202 Accepted - [\*]：表示一个请求已经进入后台排队（异步任务）*
  - 204 NO CONTENT - [DELETE]：用户删除数据成功。
  - 400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
  - 401 Unauthorized - [\*]：表示用户没有权限（令牌、用户名、密码错误）。
  - 403 Forbidden - [\*]：表示用户得到授权（与401错误相对），但是访问是被禁止的。
  - 404 NOT FOUND - [\*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
  - 406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
  - 410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
  - 422 Unprocesable entity - [POST/PUT/PATCH]：当创建一个对象时，发生一个验证错误。
  - 500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

- 错误处理，状态码是4xx时，应返回错误信息，error当做key。

  ```
  {
  	error: "Invalid API key"
  }
  ```

- 返回结果，针对不同操作，服务器向用户返回的结果应该符合以下规范。

  - GET  /collection：返回资源对象的列表（数组）

  - GET  /collection/resource：返回单个资源对象

  - POST  /collection：返回新生成的资源对象
  - PUT  /collection/resource：返回完整的资源对象
  - PATCH  /collection/resource：返回完整的资源对象
  - DELETE  /collection/resource：返回一个空文档

- Hypermedia API，RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。

  ```
  {
  	"link":{
  		"rel": "collection https://www.example.com/zoos",
  		"href": "https://api.example.com/zoos",
  		"title": "List of zoos",
  		"type": "application/vnd.yourformat+json",
  	}
  }
  ```

  

## 认证

#### 原理：

**用户登陆后生成 token，后续请求需校验 token**

##### 定义数据库字段

```python
#models
from django.db import models

class UserInfo(models.Model):
    user_type_choices = (
        (1, '普通用户'),
        (2, 'VIP'),
        (3, 'SVIP'),
    )
    user_type = models.IntegerField(choices=user_type_choices)
    username = models.CharField(max_length=32, unique=True)
    password = models.CharField(max_length=64)

class UserToken(models.Model):
    user = models.OneToOneField(to='UserInfo', on_delete=models.CASCADE)
    token = models.CharField(max_length=64)

```

##### 同步数据库

生成数据库迁移：`python manage.py makemigrations`

数据库建表：`python manage.py migrate`

##### 创建登录视图生成 token

```python
#views
from django.http import JsonResponse
from app1 import models

def md5(user):
    import hashlib
    import time

    ctime = str(time.time())
    m = hashlib.md5(bytes(user, encoding='utf-8'))
    m.update(bytes(ctime,encoding='utf-8'))
    return m.hexdigest()


class LoginView(APIView):
    def post(self, request, *args, **kwargs):
        ret = {'code':1000, 'msg':None}
        try:
            user = request._request.POST.get('username')
            pwd = request._request.POST.get('password')
            print(user, pwd)
            obj = models.UserInfo.objects.filter(username=user, password=pwd).first()
            if not obj:
                ret['code'] = 1001
                ret['msg'] = '用户名或密码失败'
            #为登录用户创建token
            token = md5(user)
            #存在就更新，不存在就创建
            models.UserToken.objects.update_or_create(user=obj, defaults={'token':token})
            ret['token'] = token
        except Exception as e:
            ret['code'] = 1002
            ret['msg'] = '请求异常'
        return JsonResponse(ret)
```

#### 1、自定义认证类

自定义认证类需继承 `BaseAuthentication` ，并实现 `authenticate` 方法和 `authenticate_header` 方法

```python
from rest_framework.authentication import BaseAuthentication

class Authentication(BaseAuthentication):
    def authenticate(self, request):
        token = request._request.GET.get('token')
        token_obj = models.UserToken.objects.filter(token=token).first()
        #获取用户名和密码，去数据库校验
        if not token:
            raise exceptions.AuthenticationFailed('用户认证失败')
        return (token_obj.user, token_obj)

    def authenticate_header(self, request):
        pass
```

视图函数中指定使用的认证类

```python
class AuthView(APIView):
    #指定使用的认证类
    authentication_classes = [Authentication,]

    def get(self, request, *args, **kwargs):
        ret = {'code':1000, 'msg':None}
        try:
            ret['msg'] = '访问成功'
        except Exception as e:
            pass
        return JsonResponse(ret)
```

#### 2、内置认证类

`rest_framework` 在 `authentication` 中为我们提供了几个认证类：

- `BaseAuthentication`
- `BasicAuthentication`
- `SessionAuthentication`
- `TokenAuthentication`
- `RemoteUserAuthentication`

#### 3、全局配置/局部配置

##### 全局配置：

配置文件中配置默认使用的版本类列表 ***DEFAULT_AUTHENTICATION_CLASSES***

认证失败的 `user` 信息 ***UNAUTHENTICATED_USER***

认证失败的 `auth` 信息 ***UNAUTHENTICATED_TOKEN***

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES':['rest_framework.authentication.BaseAuthentication',],
}
```

##### 局部配置：

视图类中指定使用版本类列表 ***authentication_classes***： 

```python
class AuthView(APIView):
    #指定使用的认证类
    authentication_classes = [BaseAuthentication,]
```

#### 4、源码流程

`APIView.dispatch` -> `APIView.initial` -> `APIView.perform_authentication` -> `Request.user` -> `Reqeust._authenticate` -> `authenticator.authenticate`



## 权限

### 原理：

**根据用户等级不用可以访问不同的接口**

### 1、自定义权限类

自定义权限类需继承 `BasePermission`，并实现 `has_permission` 方法

#### 情况1：视图中指定认证类

此时可以通过 `request.user` 获取用户信息，失败后返回自定义  `message`  信息

```python
from rest_framework.permissions import BasePermission

class Permission(BasePermission):
    message = '访问权限不够'  #失败后返回信息
    def has_permission(self,request,view):
        if request.user.user_type != 3:
            return False
        return True
```

视图中指定使用权限类

```python
class PermView(APIView):
    authentication_classes = [Authentication,]
    #指定权限类
    permission_classes = [Permission,]

    def get(self, request, *args, **kwargs):
        # if request.user.user_type <3 :
        return HttpResponse(json.dumps({'msg':'访问成功'}))

```

#### 情况2：视图中不指定认证类

此时需根据 `token` 获取用户信息，失败返回 `Authentication credentials were not provided` 信息

```python
from rest_framework.permissions import BasePermission

class Permission(BasePermission):
    message = '权限不够'  #失败后返回信息
    def has_permission(self,request,view):
        token = request._request.GET.get('token')
        obj = models.UserToken.objects.filter(token=token).first()
        if obj.user.user_type !=3:
        # if request.user.user_type != 3:
            return False
        return True
```

视图中指定权限类

```python
class PermView(APIView):
    # authentication_classes = [Authentication,]
    #指定权限类
    permission_classes = [Permission,]

    def get(self, request, *args, **kwargs):
        # if request.user.user_type <3 :
        return HttpResponse(json.dumps({'msg':'访问成功'}))
```

### 2、内置权限类

`rest_framework` 在 `permissions` 中内置了几个权限类：

- `BasePermission`
- `AllowAny`
- `IsAuthenticated`
- `IsAdminUser`
- `IsAuthenticatedOrReadOnly`
- `DjangoModelPermissions`
- `DjangoModelPermissionsOrAnonReadOnly`
- `DjangoObjectPermissions`

### 3、全局配置/局部配置

##### 全局配置：

配置文件中配置默认使用的权限类列表 ***DEFAULT_PERMISSION_CLASSES***

```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES':['rest_framework.permissions.BasePermission',],
}
```

##### 局部配置

视图类中指定使用的权限类列表 ***permission_classes***

```python
class PermView(APIView):
    # authentication_classes = [BaseAuthentication,]
    permission_classes = [BasePermission,]
```

另外，权限类中自定义 ***message*** 作为访问异常的返回信息（需指定认证类）

### 4、源码流程

`APIView.dispatch` -> `APIView.initial` -> `APIView.check_permissions` -> `APIView.get_permissions` -> `permission.has_permission` 

## 节流

### 原理：

记录用户的访问记录，并根据一定时间的访问次数进行限制

### 1、自定义节流类

自定义节流类需继承 `BaseThrottle`，并实现 `allow_request` 和 `wait` 方法

```python
from rest_framework.throttling import BaseThrottle
import time
Throttle_dict={}

class VisitThrottle(BaseThrottle):
    def __init__(self):
        self.history=None

    def allow_request(self,request, view):
        remote_addr = request.META.get('REMOTE_ADDR')
        ctime = time.time()
        if remote_addr not in Throttle_dict:
            Throttle_dict[remote_addr]=[ctime,]
            return True
        history = Throttle_dict.get(remote_addr)
        self.history=history
        while history and history[-1]<ctime-60:
            history.pop()
        if len(history) <3:
            history.insert(0, ctime)
            return True

    def wait(self):
        ctime = time.time()
        return 60 - (ctime-self.history[-1])
```

视图类中指定节流类

```python
class ThrottleView(APIView):
    #指定节流类
    throttle_classes = [VisitThrottle,]

    def get(self, request, *args, **kwargs):
        return HttpResponse(json.dumps({'msg':'访问成功'}))

```

### 2、内置节流类

`rest_framework` 在 `throttling.py` 中内置了几个节流类：

- `BaseThrottle`
- `SimpleRateThrottle`
- `AnonRateThrottle`
- `UserRateThrottle`
- `ScopedRateThrottle`

### 3、全局配置/局部配置

##### 全局配置

配置文件中配置默认使用的节流类列表 ***DEFAULT_THROTTLE_CLASSES***

```python
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_CLASSES':['rest_framework.throttling.BaseThrottle',],
}
```

##### 局部配置

视图类中指定使用的节流类列表 ***throttle_classes***

```python
from rest_framework.throttling import BaseThrottle

class ThrottleView(APIView):
    throttle_classes = [BaseThrottle,]
```

### 4、源码流程

`APIView.dispatch` -> `APIView.initial` -> `APIView.check_throttles` ->  `APIView.get_throttles` -> `throttle.allow_request` 


## 版本控制

### 一、基于 URL 的 GET 传参

例如：**http://127.0.0.1:8000/api/user/?version=v1**

#### 1、通过 request 获取传递的版本参数

- `version = request._request.GET.get('version')`
- `version = request.query_params.get('version')`

`query_params` 等价于 `_request.GET`

#### 2、自定义版本类

自定义版本类中需实现 `determine_version` 方法，并在视图函数中用 *versioning_class* 指定自定义的版本类

```python
# views

class Paramversion(object):
    # 实现 determine_version 方法
	def determine_version(self, request, *args, **kwargs): 
        version = request.query_params.get('version')
        return version
    
class UsersView(APIView):
    versioning_class = Paranversion  # 指定自定义的版本类
    def get(self, request, *args, **kwargs):
        # version = request._request.GET.get('version')
        # version = request.query_params.get('version')
        print(request.version)
        return HttpResponse('用户列表')
```

#### 3、使用内置的版本类 `QueryParameterVersioning` 

`rest_framework.versioning` 中内置了 `QueryParameterVersioning` 版本类，能够处理通过 get 方法传递的版本参数

导入 `QueryParameterVersioning` 类，并在视图函数中 *versioning_class* 参数指定内置类

```python
# views

from rest_framework.versioning import QueryParameterVersioning
    
class UsersView(APIView):
    versioning_class = QueryParameterVersioning #指定内置类
    def get(self, request, *args, **kwargs):
        # version = request._request.GET.get('version')
        # version = request.query_params.get('version')
        print(request.version)
        return HttpResponse('用户列表')
```

`QueryParameterVersioning` 类实现了 `determine_version`方法，源码如下

```python
def determine_version(self, request, *args, **kwargs):
    version = request.query_params.get(self.version_param, self.default_version)
    if not self.is_allowed_version(version):
        raise exceptions.NotFound(self.invalid_version_message)
    return version
```

`determine_version` 方法尝试获取 `request.query_params` 键为 *version_param* 的值，如果获取不到则为默认值 *default_version*，并且版本必须得为允许得值，即 *allowed_version*。通过查看源码，这三个参数可以通过全局配置。

- *version_param* ：URL 中传递的参数键
- *default_version* ：未传递参数或传递的参数与 ***VERSION_PARAM*** 不一致时，默认版本
- *allowed_version* ：允许传递的版本范围

```python
# settings

REST_FRAMEWORK ={
    "DEFAULT_VERSION":'V1', #不传参默认值
    "ALLOWED_VERSION":['V1','V2'], #允许传递的版本
    "VERSION_PARAM":'version'， #传参key
}
```

如果在全局配置了 ***DEFAULT_VERSIONING_CLASS*** 参数，则视图函数中不需要再指定 *versioning_class* 参数

```python
# settings

REST_FRAMEWORK ={
    "DEFAULT_VERSIONING_CLASS":'rest_framework.versioning.QueryParameterVersioning',
}
```



### 二、基于 URL 的正则方式（推荐）

例如：**http://127.0.0.1:8000/api/v1/user/**

#### 使用内置的版本类 `URLPathVersioning` 

`rest_framework.versioning` 中内置了 `URLPathVersioning` 类，能够处理版本放在 URL 中的情况

导入`URLPathVersioning` 类，并在视图函数中 *versioning_class* 参数指定内置类

```python
# views

from rest_framework.versioning import URLPathVersioning 
    
class UsersView(APIView):
    versioning_class = URLPathVersioning 
    def get(self, request, *args, **kwargs):
        # version = request._request.GET.get('version')
        # version = request.query_params.get('version')
        print(request.version)
        return HttpResponse('用户列表')
```

urls 中路由需要设置为如下正则，`<>` 中参数需和配置文件中 ***VERSION_PARAM*** 一致，否则无法获取。URL 中版本必须和正则中的版本一致，否则报错

```python
# urls

urlpatterns=[
    url(r'^(?P<version>[v1|v2]+)/user/$',views.UsersView.as_view()),
]
```

`URLPathVersioning` 类同样实现了 `determine_version`方法，源码如下

```python
def determine_version(self, request, *args, **kwargs):
    version = kwargs.get(self.version_param, self.default_version)
    if version is None:
        version = self.default_version

    if not self.is_allowed_version(version):
        raise exceptions.NotFound(self.invalid_version_message)
    return version
```

可以看到 `URLPathVersioning` 与 `QueryParameterVersioning` 类似，只是从 ***kwargs*** 中获取参数。同样可以再配置文件之配置对应参数

```python
# settings

REST_FRAMEWORK ={
    "DEFAULT_VERSION":'V1', #不传参默认值
    "ALLOWED_VERSION":['V1','V2'], #允许传递的版本
    "VERSION_PARAM":'version'， #传参key
}
```

### 三、基于命名空间 namespace  

该方式是将版本信息放在主 `urls` 的 各 `app `分路由的 `namespace` 中，无需放在 `url `中 或通过 `get `传参。`rest_framework` 中内置了处理该情况的版本类 `NamespaceVersioning`

#### 使用内置的版本类 `NamespaceVersioning`

视图函数中指定 `NamespaceVersioning` 类

```python
# views

from rest_framework.versioning import NamespaceVersioning 
    
class UsersView(APIView):
    versioning_class = NamespaceVersioning 
    def get(self, request, *args, **kwargs):
        # version = request._request.GET.get('version')
        # version = request.query_params.get('version')
        print(request.version)
        return HttpResponse('用户列表')
```

主路由设置 `namespace` 指定版本信息

```python
# urls

urlpatterns = [
    path('admin/', admin.site.urls),
    url('^api/', include('app1.urls', namespace='v3')),
]
```

分路由需要设置 `app_name` ，否则抛异常：`django.core.exceptions.ImproperlyConfigured`

```python
from django.conf.urls import url
from app1 import views

app_name = 'app1'  # 必须设置，否则抛异常

urlpatterns = [
    url(r'^version/', views.VersionView.as_view(), ),
]

```

`NamespaceVersioning` 类中也实现了 `determine_version` 方法，源码如下

```python
def determine_version(self, request, *args, **kwargs):
    resolver_match = getattr(request, 'resolver_match', None)
    if resolver_match is None or not resolver_match.namespace:
        return self.default_version

    # Allow for possibly nested namespaces.
    possible_versions = resolver_match.namespace.split(':')
    for version in possible_versions:
        if self.is_allowed_version(version):
            return version
    raise exceptions.NotFound(self.invalid_version_message)
```

`determine_version` 利用反射获取 `resolver_match` 属性，如果获取不到或者未设置 `namespace`，则返回默认版本（如果设置了 `app_name` 则返回 `app_name`）。

然后利用 `split` 分割出可能的版本值，并遍历可能的版本值进行校验，如果在允许范围内则返回。注意：如果没有设置 ***ALLOWED_VERSION***，根据 `is_allowed_version` 源码直接返回 **True**，将直接返回第一个可能的版本，这可能会有异常；例如：`namespace='app1:v1'`，未设置 ***ALLOWED_VERSION*** 时，返回的版本将为 `app1`，这显然不是我们想要返回的版本信息

因此，最好在配置文件配置 ***DEFAULT_VERSION*** 和 ***ALLOWED_VERSION***。

```python
# settings
REST_FRAMEWORK ={
    "DEFAULT_VERSION":'V1', #不传参默认值
    "ALLOWED_VERSION":['V1','V2'], #允许传递的版本
    "VERSION_PARAM":'version'， #传参key
}
```

### 四、配置文件设置版本类

APIView 类中参数 versioning_class 指定了版本类，通过配置文件可以设置默认的版本类，视图中无需在设置 *versioning_class* 参数

```python
# views

class UsersView(APIView):
    def get(self, request, *args, **kwargs):
        # version = request._request.GET.get('version')
        # version = request.query_params.get('version')
        print(request.version)
        return HttpResponse('用户列表')
```

配置文件

```python
# settings

REST_FRAMEWORK ={
    #默认使用的版本类
    "DEFAULT_VERSIONING_CLASS":'rest_framework.versioning.URLPathVersioning',
    "DEFAULT_VERSION":'V1', #不传参默认值
    "ALLOWED_VERSION":['V1','V2'], #允许传递的版本
    "VERSION_PARAM":'version'， #传参key
}
```

## 解析器

### 前戏

Django 中的 request.POST / request.body

request.body 有值时，request.POST 不一定有值

request.POST 有值条件

1、请求头要求

如果请求头中的 Content-Type: application/x-www-form-urlencoded，request.POST中才有值（去 request.body中解析数据）

2、数据格式要求

name=alex&age=18&gender=男

如：

- form表单：默认符合上述两个条件
- ajax提交：
  - 

### rest_framework 解析器

视图类中添加

```python
parser_classes = [JSONParser, FormParser, ]
```



#### JSONParser

只能解析 Content-Type:application/json 头

#### FormParser

解析 Content-Type: application/x-www-form-urlencoded 头





## 序列化

### 源码流程

`BaseSerializer` 序列化基类的构造方法`__new__` 方法中，根据序列化类实例化时传递的参数 *many* 不同执行不同的方法

```python
def __new__(cls, *args, **kwargs):
    # We override this method in order to automagically create
    # `ListSerializer` classes instead when `many=True` is set.
    if kwargs.pop('many', False):
        # many=True, 对 QuerySet进行处理
        return cls.many_init(*args, **kwargs)
    #many=False, 对单个对象进行处理
    return super().__new__(cls, *args, **kwargs)
```

- *many* = True，表示序列化对象为 QuerySet 对象。执行类本身的 `many_init` 方法，该方法中会尝试获取序列化类 `list_serializer_class` ，并返回其实例化对象；如果获取不到则默认为 `ListSerializer`。即序列化时使用 `ListSerializer` 类

  ```python
  def many_init(cls, *args, **kwargs):
  		...
          meta = getattr(cls, 'Meta', None) 
          list_serializer_class = getattr(meta, 'list_serializer_class', ListSerializer) #获取序列化类
          return list_serializer_class(*args, **list_kwargs) #实例化序列化类
  ```

- *many* =False 或 None，序列化对象为单个对象，则执行父类的构造方法，即使用默认的`Serializer` 类

调用序列化对象的 `data` 方法可以获取序列化后的数据，查看源码发现不论是 `Serializer` 类还是 `ListSerializer` 类的 `data` 方法都继承自父类 `BaseSerializer`

`Serializer` 类

```python
@property
def data(self):
    ret = super().data
    return ReturnDict(ret, serializer=self)
```

`ListSerializer` 类

```python
@property
def data(self):
    ret = super().data
    return ReturnList(ret, serializer=self)
```

父类 `BaseSerializer` 的 `data` 方法中回调了序列化类的 `to_representation` 方法

`to_representation` 方法接收 *instance* 参数（数据库object对象），并将其返回值赋值给`_data`，作为 `data` 方法的返回值

```python
@property
def data(self):
    if hasattr(self, 'initial_data') and not hasattr(self, '_validated_data'):
        msg = (
            'When a serializer is passed a `data` keyword argument you '
            'must call `.is_valid()` before attempting to access the '
            'serialized `.data` representation.\n'
            'You should either call `.is_valid()` first, '
            'or access `.initial_data` instead.'
        )
        raise AssertionError(msg)
	if not hasattr(self, '_data'):
		if self.instance is not None and not getattr(self, '_errors', None):
			self._data = self.to_representation(self.instance)
        elif hasattr(self, '_validated_data') and not getattr(self, '_errors', None):
			self._data = self.to_representation(self.validated_data)
		else:
			self._data = self.get_initial()
	return self._data
```

序列化类 `Serializer` 类的 `to_representation` 方法中，遍历数据库中定义的字段，并通过 `fields.get_attribute` 方法获取 `instance` 对象的值，

```python
def to_representation(self, instance):
    """
    Object instance -> Dict of primitive datatypes.
    """
    ret = OrderedDict()
    fields = self._readable_fields
    #数据库定义的字段
    for field in fields:
        try:
            # serializer.CharField.get_attribute(对象) #对象.username
            attribute = field.get_attribute(instance)
        except SkipField:
            continue

        # We skip `to_representation` for `None` values so that fields do
        # not have to explicitly deal with that case.
        #
        # For related fields with `use_pk_only_optimization` we need to
        # resolve the pk value.
        check_for_none = attribute.pk if isinstance(attribute, PKOnlyObject) else attribute
        if check_for_none is None:
            ret[field.field_name] = None
        else:
            ret[field.field_name] = field.to_representation(attribute)

    return ret
```



