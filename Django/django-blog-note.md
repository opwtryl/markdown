# Django博客笔记

[toc]

## 一、前言

本文为 [追梦人物的Django博客教程](https://www.zmrenwu.com/post/2/) 学习过程所记录的笔记,主要是一些由于使用 django2.0 + CentOS6 而导致与原教程不一致的地方。

## 二、搭建开发环境

1.python:

`Python 3.6.5 |Anaconda, Inc.`

2.django

`2.0.5`

3.虚拟环境

    conda create -n blog_project
    activate blog_project

4.编辑器
`Visual Studio Code`

## 三、创建 Django 博客的数据库模型

```python
class Post(models.Model):
    # Django2.0起,on_delete必填
    author = models.ForeignKey(User, on_delete=models.CASECADE)
    category = models.ForeignKey(Category, on_delete=models.CASCADE)
```

## 四、Django 博客首页视图

### 1、绑定 URL 与视图函数

**方案一:**

    from django.conf.urls import url

更改成

    from django.urls import re_path

可以继续使用原教程的URL。
如:

```python
urlpatterns = [
    re_path(r'^$', views.index, name='index'),
]
```

**方案二:**

    from django.urls import path

需要更改原教程的URL。

```python
urlpatterns = [
    path('', views.index, name='index'),
]
```

Django2.0建议使用方案二,[点击查看官网教程](https://docs.djangoproject.com/zh-hans/2.0/intro/tutorial01/)。

### 2、使用 Django 模板系统

原教程在**项目根目录**创建templates文件夹,如下：

```plaintext
blogproject\
    manage.py
    blog_project\
        __init__.py
        settings.py
        ...
    blog\
        __init__.py
        models.py
        ,,,
    templates\
        blog\
            index.html
```

Django2.0建议在**blog文件夹里创建templates文件夹，并在这个templates文件夹里再创建一个文件夹blog,然后在其中放置html文件**，无需在settings.py设置模板文件所在的路径，即：

```plaintext
blogproject\
    manage.py
    blog_project\
        __init__.py
        settings.py
        ...
    blog\
        __init__.py
        models.py
        templates\
            blog\
                index.html
        ...
```

[点击查看官网教程](https://docs.djangoproject.com/zh-hans/2.0/intro/tutorial03/)

## 五、真正的 Django 博客首页视图

    {% load staticfiles %}

改为

    {% load staticfiles %}

> [The staticfiles template tag library will be deprecated and removed in a future Django version.](https://github.com/encode/django-rest-framework/pull/5773)

## 六、博客文章详情页

blog/urls.py:

```python

from django.urls import path
from . import views

app_name = 'blog'
urlpatterns = [
    path('', views.index, name='index'),
    path('post/<int:pk>/', views.detail, name='detail'),
]
```

## 七、使用 Nginx 和 Gunicorn 部署 Django 博客

服务器系统版本信息：

    CentOS release 6.9 (Final)
    4.17.1-1.el6.elrepo.x86_64

### 1、安装软件

*以下均为在CentOS6服务器上的操作*

#### 1.1更新系统

    yum update

#### 1.2安装Anaconda

```bash
yum install wget
wget https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh
bash Anaconda3-5.2.0-Linux-x86_64.sh
source /root/.bashrc # 修改系统默认Python为Anaconda下的Python，即Python3版本，我直接在/root目录下安装的
```

#### 1.3安装nginx

```bash
vim /etc/yum.repos.d/nginx.repo  #添加nginx安装源

    [nginx]
    name=nginx repo
    baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
    gpgcheck=0
    enabled=1
```

    yum install nginx # 安装nginx

```bash
vim /etc/nginx/nginx.d/www.opwtryl.com.conf # 配置nginx

    server {
        charset utf-8;
        listen 80;
        server_name www.opwtryl.com;
        location /static/ {
            root /root/sites/www.opwtryl.com/blog_project/blog_project;
        }
        location / {
            proxy_set_header Host $host;
            proxy_pass http://unix:/tmp/www.opwtryl.com.socket;
        }
    }
```

```bash
vim /etc/nginx/nginx.conf  # 设置nginx用户属性

    user  root;# 默认为 user nginx;会出现 failed (13: Permission denied) 导致CSS加载失败
```

如果出现其他原因导致nginx无法正常工作，可以按以下步骤排查：

```bash
cat /etc/nginx/nginx.conf

    error_log: /var/log/nginx/error.log  # 找到错误日志路径

cat /var/log/nginx/error.log  # 看看具体错误信息
```

#### 1.4安装其他软件和依赖

```bash
yum install git
pip install django-haystack markdown gunicorn jieba whoosh
```

### 2、自动启动 Gunicorn

```bash
vim /etc/rc.d/init.d/gunicorn-www.opwtryl.com # 创建开机启动脚本

    #!/bin/sh
    # chkconfig:35 80 70
    # description: nginx gunicorn开机自启动

    service nginx start
    cd /root/sites/www.opwtryl.com/blog_project/blog_project
    /root/anaconda3/bin/gunicorn --bind unix:/tmp/www.opwtryl.com.socket blog_project.wsgi:application
```

    chkconfig --add  gunicorn-www.opwtryl.com  # 添加开机启动服务

### 3、创建用户

```bash
useradd -m -s /bin/bash opwtryl
usermod -a -G wheel opwtryl
passwd opwtryl
```

```bash
visudo  # 解决 is not in the sudoers file 报错

    root ALL=(ALL) ALL # 找到到这一行,在下面添加一行
    xxx  ALL=(ALL) ALL # 其中xxx是你要加入的用户名称
```

## 八、使用 Fabric 自动化部署

```python
pip install fabric3  # 支持python3
```
新建`fabfile.py`文件:
```python
from fabric.api import env, run
from fabric.operations import sudo

GIT_REPO = 'yuor git repository'
env.user = 'your host username'
env.password = 'your host password'

env.hosts = ['your domain']
env.port = 'your vps ssh port'
def deploy():
    source_folder = 'your source folder'
    run('cd {} && git pull '.format(source_folder))
    run('''
        cd {} && 
        /root/anaconda3/bin/python install -r requirement.txt &&
        /root/anaconda3/bin/python manage.py collectstatic --noinput &&
        /root/anaconda3/bin/python manage.py migrate &&
        '''.format(source_folder))

    sudo('''
        cd {} &&
        /root/anaconda3/bin/gunicorn --bind unix:/tmp/www.opwtryl.com.socket blog_project.wsgi:application
        '''.format(source_folder))
    sudo('service nginx reload')
```

本地执行:
```bash
fab deploy
```