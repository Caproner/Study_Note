# Django项目部署

记录一下这两天学到的规范化Django项目部署

## 创建空Django项目

### 步骤

前面绝大多数操作都是在服务器上完成

+ 创建一个普通用户用于管理Django项目
  + 这里使用的用户名为`caproner`
  + 不需要什么特别的权限，建就完事了
  + 就算以后需要也只需要由`root`赋一下就行了
+ 构建Django项目目录
  + 切换到普通用户，并到用户根目录
  + 创建`data`文件夹
    + `data`目录下存放所有项目相关的东西
  + 在`data`文件夹中创建`code`文件夹与`release`文件夹
    + `code`文件夹放源代码，`release`放运行版本
+ 新建Django项目并同步到github
  + 在github上新建一个空的项目
    + 这里使用的项目名为`PersonalWeb`
    + 这个项目就是之后的Django项目
  + 回到服务器的`code`目录，`git clone`这个项目
  + 进入`PersonalWeb`文件夹，创建Django项目
    + 命令为`django-admin startproject PersonalWeb`
  + 将创建好的Django项目的`PersonalWeb`文件夹链接到`release`里
    + 命令为`ln -s ~/data/code/PersonalWeb/PersonalWeb ~/data/release/PersonalWeb`
    + 于是以后直接在`release/PersonalWeb`下操作就行
  + `git push`同步上去，在开发机`git pull`一份

### 说明

创建好之后的项目架构如下图所示：

<img src=img/DjangoBegining/1.png />

+ `manage.py`：Django项目的主要命令工具
+ `PersonalWeb/settings.py`：Django项目的后台配置
+ `PersonalWeb/urls.py`：Django项目的后台路由配置
  + 所谓路由，其实就是管理url分发，换句话说就是，对不同的url调用不同的函数or功能
+ `PersonalWeb/wsgi.py`：不太清楚，大概是用于配置WSGI接口的？

## 写一个Hello World项目

### 步骤

这里的步骤中，前两步都是在开发机上完成，最后一步【运行项目】是在服务器上完成。

#### 前置配置

+ 将项目中小的`PersonalWeb`文件夹改为`mysite`
  + 准确的说是修改`PersonalWeb`包为`mysite`包，这么做是为了区别于项目名
+ 搜索项目文件夹中所有代码中包含`PersonalWeb.`的字段，全部替换为`mysite.`
  + 因为初始项目中已经有许多调用`PersonalWeb`包中函数的代码，而这个包已经被我们改名为`mysite`了，所以需要做相应替换
  + 注意字段是以`.`结尾的，不要看漏了
+ 在`PersonalWeb`文件夹下新建文件夹，名为`frontend`
  + 这个文件夹与`mysite`同级
  + 这里存放Django项目本身的代码，而`mysite`存放Django项目的配置
+ 在`frontend`文件夹中创建三个文件：`__init__.py`，`urls.py`，`views.py`
  + `urls.py`是配置项目本身路由的地方
  + `views.py`是处理逻辑的地方
+ 在`mysite`中注册`frontend`包
  + 打开`mysite/settings.py`，找到下图的代码，并添加`frontend`进去（如红框所示）

<img src=img/DjangoBegining/2.png />

+ 配置`mysite`的路由，将其全部转发到`frontend`进去
  + 打开`mysite/urls.py`，找到下图的代码，并添加红框中的代码进去（针对url和include的解释详见说明部分）

<img src=img/DjangoBegining/3.png />

+ 打开`mysite/settings.py`，将变量`DATABASES`改为如下代码：

```python
DATABASES = {
    'default': {}
}
```

+ 设置`frontend`的理由：打开`frontend/urls.py`，在其中加上如下代码：

```python
# -*- coding: utf-8 -*-
from django.conf.urls import url
from .views import hello_world


urlpatterns = [
    url(r'^$', hello_world, name="hello_world")
]
```

#### 开始写Hello World本体

打开`frontend/views.py`，并写如下代码：

```python
# -*- coding: utf-8 -*-
from django.http import HttpResponse


def hello_world(request):
    return HttpResponse('hello world')
```

然后保存，`git push`，并在服务器上`git pull`。

#### 运行项目

回到服务器上。

+ 到`release/PersonalWeb`上，运行`python manage.py`
  + 如果不报错就可以进入下一步，报错的话就根据错误提示debug或补包。下同。
+ 运行`python manage.py check`
+ 运行`python manage.py runserver`
  + 默认运行在8000，不过腾讯云上的话需要用nginx转发才能访问。
  + 这边配置了nginx将2333的访问转8000，于是便是使用`127.0.0.1/2333`访问。
+ 尝试访问，能看到hello world的话就大功告成了。

### 说明

#### 关于路由配置中的url

该函数用于表示【在遇到怎样的url的时候，调用什么东西】。

例如说`mysite/urls.py`里的这句：

```python
url(r'', include('frontend.urls'))
```

前面表示匹配规则，后面表示匹配到了的话要做什么。

这里的意思就是，匹配到空字符串的话，就转到`frontend.urls`里去。

匹配到空字符串其实就等价于【啥都行】。这句的意思就是无条件将url转发到`frontend`的路由配置中。

#### 关于Hello World本体

其实就是response一个“hello world”的字符串而已。

## 写一个带网页的Hello World

### 步骤

回到开发机。

+ 在`frontend`文件夹下创建`templates`
  + `templates`用于存放html模板文件，也就是带模板的html
+ 在`templates`下创建一个`hello_world.html`，内容如下：

```django
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Hello world</title>
	</head>
	<body>
		{{ value }}
	</body>
</html>
```

+ 进入`frontend/views.py`，修改代码为如下：

```python
# -*- coding: utf-8 -*-
from django.http import HttpResponse
from django.views.generic.base import View
from django.shortcuts import render


# 加了这个类，以及上面的两个包，在说明里会说下
class HelloWorldView(View):
    def get(self, request):
        return render(request, "hello_world.html", dict(value="hello world"))

def hello_world(request):
    return HttpResponse('hello world')
```

+ 进入`frontend/urls.py`，修改路由配置为如下：

```python
# -*- coding: utf-8 -*-
from django.conf.urls import url
from .views import hello_world, HelloWorldView  # 新加了一个import


urlpatterns = [
    url(r'^$', HelloWorldView.as_view(), name="hello_world")
]
```

然后使用`git`同步到服务器并运行项目，便可以看到效果。

### 说明

#### html模板

在Django中，html可以带一些可替换元素，例如上面的`{{ value }}`，这些会在用户打开网页的时候由后端对其进行替换。

#### get请求

在`views.py`中，继承了`View`类的自定义View中可以重载`get`函数以处理get请求。

在上面其使用了`render`函数返回了`hello_world.html`文件并将其`value`渲染为字符串`hello world`。

#### 路由配置中的路径匹配

这里使用的正则匹配是`r'^$'`，其表示**仅匹配空字符串**。

## 对请求的简单处理

这里要做到的效果是，针对诸如如下格式的请求：

```http
ip:port/id/name
```

可以解析出`id`和`name`并在网页中显示。

### 步骤

+ 修改路由配置，将`frontend/urls.py`改为如下：

```python
# -*- coding: utf-8 -*-
from django.conf.urls import url
from .views import hello_world, HelloWorldView


urlpatterns = [
    url(r'^(?P<id>\d+)/(?P<docs>[a-zA-Z\u4E00-\u9FA5]*)$', HelloWorldView.as_view(), name="hello_world")
]
```

+ 修改后端渲染，将`frontend/views.py`改为如下：

```python
# -*- coding: utf-8 -*-
from django.http import HttpResponse
from django.views.generic.base import View
from django.shortcuts import render
from .share.utils import safe_int


class HelloWorldView(View):
    def get(self, request, *args, **kwargs):
        docs = kwargs.get('docs')
        article_id = safe_int(kwargs.get('id'))
        return render(request, "hello_world.html", dict(id=article_id, name=docs))

def hello_world(request):
    return HttpResponse('hello world')
```

+ 在`views.py`中引用了一个函数`safe_int()`，我们需要将这个函数和其所在包补上

  + 在`frontend`文件夹中新建文件夹`share`，并在`share`中新建`__init__.py`和`utils.py`

  + 在`utils.py`中补上如下代码：

  + ```python
    # -*- coding: utf-8 -*- #
    
    
    def safe_int(src):
        """
        转成int
        """
        if not src:
            return src
        else:
            return int(src)
    ```

+ 修改`frontend/templates/hello_world.html`为如下：

```django
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Hello world</title>
    </head>
    <body>
        <a href="/1/你好">
            <span>点我传值</span>
        </a>
        <p>{{ id }}</p>
        <p>{{ name }}</p>
    </body>
</html>
```

+ 将代码同步到服务器并运行查看效果

### 说明

+ 关于新的url解析正则，其实就是提取出`id`和`name`（并且需要其满足规则）
+ `share`文件夹用于放置各种工具函数

## 监控与日志

这里要做的是部署进程管理软件suervisor，用于监控Django进程，以及使用Django的日志功能。

### 步骤

#### supervisor

直接`yum install`即可

主要命令有：

+ `supervisord -c /etc/supervisord.conf `：指定配置文件开启
+ `supervisorctl shutdown`：关闭supervisor
+ `supervisorctl status`：查看所有进程的状态
+ `supervisorctl stop es`：停止es
+ `supervisorctl start es`：启动es
+ `supervisorctl restart es`: 重启es
+ `supervisorctl update`：配置文件修改后可以使用该命令加载新的配置
+ `supervisorctl reload`: 重新启动配置中的所有程序

需要修改的配置有：

+ 打开`/etc/supervisord.conf`，将最后的`[include]`改一下（个人习惯而已）：

```
[include]
files = relative/directory/*.conf
```

+ 在`/etc/supervisord.d/`里新建文件`PersonalWeb.conf`，并添加如下内容：

```
[program:PersonalWeb]
environment=PYTHON_EGG_CACHE=/tmp/.python-eggs/,MODE=DEV
directory=/home/caproner/data/release/PersonalWeb
command=gunicorn -c gun_config.py

mysite.wsgi:application -t 1200
user=caproner
autorestart=true
redirect_stderr=true
```

#### 项目配置

+ `pip install gunicorn`
+ `pip install gevent`
+ 切换回开发机，在`PersonalWeb`目录下新建文件`gun_config.py`，然后添加如下内容：

```python
# -*- coding: utf-8 -*- #

proc_name = 'PersonalWeb'
# sync/gevent
worker_class = 'gevent'
bind = ['127.0.0.1:8000']
#bind = ['0.0.0.0:17333']
workers = 4
```

同步到服务器，然后使用supervisor启动进程`PersonalWeb`，就可以访问了。

#### 日志

+ 在开发机下的目录`frontend/share/`下新建文件`logs.py`，并添加如下内容：

```python
# -*- coding: utf-8 -*-
import logging


logger = logging.getLogger('main')
```

+ 然后追加一些log的配置到`mysite/settings.py`中（以下是追加内容）：

```python
# log setting
MAIN_LOG_FILE_PATH = os.path.join(BASE_DIR, "logs/main.log")
DJANGO_LOG_FILE_PATH = os.path.join(BASE_DIR, "logs/django.log")
MARKDOWN_LOG_FILE_PATH = os.path.join(BASE_DIR, "logs/markdown.log")

LOG_FORMAT = '\n'.join((
    '/' + '-' * 80,
    '[%(levelname)s][%(asctime)s][%(process)d:%(thread)d][%(filename)s:%(lineno)d %(funcName)s]:',
    '%(message)s',
    '-' * 80 + '/',
))

if DEBUG:
    MAIN_LOG_LEVEL = 'DEBUG'
else:
    MAIN_LOG_LEVEL = 'ERROR'

LOGGING = {
    'version': 1,
    'disable_existing_loggers': True,

    'formatters': {
        'standard': {
            'format': LOG_FORMAT,
        },
    },

    'filters': {
        'require_debug_false': {
            '()': 'django.utils.log.RequireDebugFalse'
        },
        'require_debug_true': {
            '()': 'django.utils.log.CallbackFilter',
            'callback': lambda x: DEBUG,
        }
    },
    'handlers': {
        'flylog': {
            'level': 'CRITICAL',
            'class': 'flylog.FlyLogHandler',
            'formatter': 'standard',
            'source': os.path.basename(os.path.dirname(os.path.dirname(os.path.abspath(__file__)))),
            'role_list': ['default'],
        },
        'main_file': {
            'level': MAIN_LOG_LEVEL,
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': MAIN_LOG_FILE_PATH,
            'maxBytes': 1024*1024*500,
            'backupCount': 5,
            'formatter': 'standard',
            },
        'django_file': {
            'level': 'ERROR',
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': DJANGO_LOG_FILE_PATH,
            'maxBytes': 1024*1024*500,
            'backupCount': 5,
            'formatter': 'standard',
        },
        'console': {
            'level': 'DEBUG',
            'filters': ['require_debug_true'],
            'class': 'logging.StreamHandler',
            'formatter': 'standard'
        },
        'maple_flylog': {
            'level': 'ERROR',
            'class': 'flylog.FlyLogHandler',
            'formatter': 'standard',
            'source': os.path.basename(os.path.dirname(os.path.dirname(os.path.abspath(__file__)))),
        },
        'markdown': {
            'level': MAIN_LOG_LEVEL,
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': MARKDOWN_LOG_FILE_PATH,
            'maxBytes': 1024*1024*500,
            'backupCount': 5,
            'formatter': 'standard',
        }

    },
    'loggers': {
        'django': {
            'handlers': ['django_file', 'maple_flylog'],
            'level': 'DEBUG',
            'propagate': False
        },

        'django.request': {
            'handlers': ['console'],
            'level': 'DEBUG',
            'propagate': True,
        },
        'main': {
            'handlers': ['console', 'main_file', 'flylog'],
            'level': MAIN_LOG_LEVEL,
            'propagate': False
        },
        'MARKDOWN': {
            'handlers': ['console', 'markdown', 'flylog'],
            'level': MAIN_LOG_LEVEL,
            'propagate': False
        }
    }
}
```

+ 同步到服务器，然后在服务器上的`release/PersonalWeb/`目录下新建文件夹`logs`

+ 启动，没有报错的话就完事了

### 说明

#### supervisor的配置文件

这里主要说明针对特定项目的配置文件`/etc/supervisord.d/PersonalWeb.conf`

+ `directory`：指定进程所在的目录，这里我们需要指定的是`gun_config.py`所在的目录
+ `command`：指定进程启动命令，这里使用的是`gunicorn`来启动Django
+ `user`：指定进程执行用户
+ `autorestart`：显然我们需要服务端能在crash的时候自动重启

其他的可以自行查找，选项比较多，按需取即可。

#### gun-config.py配置说明

+ `proc_name`：进程名
+ `bind = ['127.0.0.1:8000']`：绑定Django启动端口为8000

#### 如何使用日志

+ 在需要使用日志的代码引入`share.logs`文件中的`logger`函数
+ 需要打日志的话直接用`logger.debug("Debug in %s[%s]", args, kwargs)`这样的格式
  + 跟c++的`printf`很像，区别在于`%s`可以表示任何类型