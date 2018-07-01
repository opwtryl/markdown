# 上传图片到七牛的N种方法

本文收集了上传图片到[七牛](https://www.qiniu.com/)的N种方法。

## 1、管理控制台上传

![管理控制台上传](http://pajv5izbx.bkt.clouddn.com/管理控制台上传.png)

[官网文档](https://developer.qiniu.com/kodo/manual/1233/console-quickstart)

## 2、Windows客户端(QsunSync)

![qiniu-Windows客户端QsunSync](http://pajv5izbx.bkt.clouddn.com/qiniu-Windows客户端QsunSync.png)

[下载地址](https://github.com/qiniu/QSunSync)

## 3、命令行工具(qshell)

![qshell](http://pajv5izbx.bkt.clouddn.com/qshell.png)

[官方文档](https://github.com/qiniu/qshell)

## 4、Python SDK

![Python SDK](http://pajv5izbx.bkt.clouddn.com/PythonSDK.png)

注意：文件名不能为 `qiniu.py`，不然会报错 `cannot import name Auth`,
因为会把`from qiniu import Auth`中的`qiniu`误判为`qiniu.py`而不是`qiniu`这个package。

[官方文档](https://developer.qiniu.com/kodo/sdk/1242/python)

## 5、MPic-图床神器

MPic-图床神器支持截图自动上传和复制图片自动上传。在通知栏MPic图标上鼠标右键单击，即可设置开启截图上传和复制图片自动上传功能。

![qiniu-MPic-图床神器](http://pajv5izbx.bkt.clouddn.com/qiniu-MPic-图床神器.png)

![qiniu-MPic-图床神器-截图上传](http://pajv5izbx.bkt.clouddn.com/qiniu-MPic-图床神器-截图上传.png)

[下载地址](http://mpic.lzhaofu.cn/)

## 6、VS Code插件

用VS Code 来写markdown文档，可以通过一些插件来实现快捷上传图片到七牛，并获得外链。

### 6.1 Markdown Preview Enhanced

在编辑页右键可打开预览页，在预览页右键即可打开Image Helper。

![qiniu-Markdown Preview Enhanced](http://pajv5izbx.bkt.clouddn.com/qiniu-Markdown%20Preview%20Enhanced.png)
![qiniu-Markdown Preview Enhanced2](http://pajv5izbx.bkt.clouddn.com/qiniu-Markdown%20Preview%20Enhanced2.png)

[官方文档](https://shd101wyy.github.io/markdown-preview-enhanced/#/zh-cn/)
### 6.2 qiniu-upload-image

![qiniu-qiniu-upload-image](http://pajv5izbx.bkt.clouddn.com/qiniu-qiniu-upload-image.gif)
[官方文档](https://github.com/yscoder/vscode-qiniu-upload-image)

