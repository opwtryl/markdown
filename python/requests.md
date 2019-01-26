# Selenium+Requests 模拟登陆

## 基础知识

### 1.request

[详细资料：requests API](http://cn.python-requests.org/zh_CN/latest/api.html)

参数 | 含义
-|-
method | 请求的方法,包括get,post,put,delete,head,options,patch
url | 请求的地址
params|请求的Query String Parameters，可用来组成url
data | 请求的payload
json|请求的 json形式的payload
headers|请求头
cookies|请求的cookies
proxies|设置代理
auth|身份验证

### 2.method

[详细资料：MDN HTTP 请求方法](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)

方法 | 含义
---------|----------
 get | 请求一个指定资源的表示形式. 使用GET的请求应该只被用于获取数据
 head | 请求一个与GET请求的响应相同的响应，但没有响应体
 post | 将实体提交到指定的资源，通常导致状态或服务器上的副作用的更改
 put|用请求有效载荷替换目标资源的所有当前表示
 delete|删除指定的资源
 options|查询服务器支持哪些Http请求方法
 patch|用于对资源应用部分修改

### 3.headers

[详细资料：MDN HTTP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

参数 | 含义
---------|----------
 Accept | 接受的响应媒体类型
 Connection|决定当前的事务完成后，是否会关闭网络连接
 Content-Encoding|告知客户端应该怎样解码才能获取在 Content-Type 中标示的媒体类型内容
 Content-Type|客户端告诉服务器实际发送的数据类型，服务器告诉客户端实际返回的内容的内容类型
 Host|指明了服务器的域名
 Keep-Alive|允许消息发送者指示连接的状态，还可以用来设置超时时长和最大请求数
 Origin|指示了请求来自于哪个站点
 Referer|表示当前页面是通过此来源页面里的链接进入的
 Strict-Transport-Security|告诉浏览器只能通过HTTPS访问当前资源，而不是HTTP
 Transfer-Encoding|仅应用于两个节点之间的消息传递，如果想要将压缩后的数据应用于整个连接，那么使用端到端传输消息首部  Content-Encoding
 User-Agent|发起请求的用户代理软件的应用类型、操作系统、软件开发商以及版本号
 X-Requested-With|判断一个请求是传统的HTTP请求，还是Ajax请求

### 4.cookies

[详细资料：MDN HTTP Set-Cookie](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie)

参数 | 含义
---------|----------
Name | 要创建或覆盖的cookie的名字  
Value | cookie的值
Domain| 指定了哪些主机可以接受Cookie。如果不指定，默认为当前文档的主机（不包含子域名）。如果指定了Domain，则一般包含子域名。
Path|指定了主机下的哪些路径可以接受Cookie（该URL路径必须存在于请求URL中）。以字符 %x2F ("/") 作为路径分隔符，子路径也会被匹配。
Expires|cookie 的最长有效时间，形式为符合 HTTP-date 规范的时间戳。
Max-Age|在 cookie 失效之前需要经过的秒数。如果Expires 和Max-Age均存在，那么 Max-Age 优先级更高。
Secure|标记为 Secure 的Cookie只应通过被HTTPS协议加密过的请求发送给服务端。
HttpOnly|为避免跨域脚本 (XSS) 攻击，通过JavaScript的 Document.cookie API无法访问带有 HttpOnly 标记的Cookie

## 具体实现

```python
import requests
import json
from selenium import webdriver

class ZhiLian():
    def __init__(self):
        self.driverExecutablePath="F:\selenium webdriver\chromedriver.exe" # chrome浏览器驱动文件路径
        self.loginUrl="https://passport.zhaopin.com/login" # 智联登录地址
        self.cookiesFilePath="zhilianCookies.json" # cookie本地保存地址
        self.s=requests.Session() # requests的 Session类可以提供 cookie持久化和保存 headers等设置，不需要每次request都填写
        self.s.headers={
                "User-Agent": "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36",
                }
        self.initSession()

    def manualLogin(self):

        driver = webdriver.Chrome(executable_path=self.driverExecutablePath)
        print("开始手动登录")
        driver.get(self.loginUrl)
        input("登录完成后按任意键继续")
        cookies=driver.get_cookies() # 获取cookies，然后保存到本地文件
        with open(self.cookiesFilePath,'w',encoding='utf-8') as f:
            json.dump(cookies,f)
        print("保存cookies完成")
        driver.quit()

    def initSession(self):
        try:
            # 读取本地cookie文件
            print("初始化开始")
            with open(self.cookiesFilePath,"r",encoding="utf-8") as f:
                cookies=json.load(f)

            # 创建cookie
            jar=requests.cookies.RequestsCookieJar()
            for cookie in cookies:
                jar.set(cookie['name'], cookie['value'],domain=cookie["domain"],path=cookie["path"],expires=2145888000) # 设置cookie 2038年过期

            self.s.cookies=jar
            r=self.s.get("https://i.zhaopin.com/") # 未登录状态下，打开 https://i.zhaopin.com/ 会跳转到 'https://www.zhaopin.com'
            assert r.url=="https://i.zhaopin.com/"
            print("初始化完成")

        # 未创建cookie本地文件会报错：FileNotFoundError
        # 创建cookie本地文件后，读取空白json文件会报错：json.decoder.JSONDecodeError
        # cookie文件的cookie信息过期后会报错：AssertionError
        except (FileNotFoundError,json.decoder.JSONDecodeError,AssertionError):
            print("初始化失败")
            self.manualLogin()
            self.initSession()

     def searchJobs(self):
        url="https://fe-api.zhaopin.com/c/i/sou"
        # 设置搜索简历条件
        params={
            "pageSize": "90",
            "cityId": "763",
            "workExperience": "-1",
            "education": "-1",
            "companyType": "-1",
            "employmentType": "-1",
            "jobWelfareTag": "-1",
            "kw": "Python",
            "kt": "3",
            "at": "c78e5dc3487f44dea5cf59afbec35dae",
            "rt": "c0d0ef9c111c40d4a0b955976a033df1",
            "_v": "0.99206062",
            "x-zp-page-request-id": "651215cb7e3f4626afc6fd8db4fcb759-1548290960245-102881",
        }
        self.s.headers.update({'Host': 'fe-api.zhaopin.com',
                             'Origin': 'https://sou.zhaopin.com',
                             'Referer': 'https://sou.zhaopin.com/?jl=763&kw=Python&kt=3'})
        r=self.s.get(url=url,params=params)
        return r.json()

    def postResume(self,jobNumber):
        # 发送简历到指定招聘职位
        url="https://fe-api.zhaopin.com/c/pc/alan/jobs/application"
        params={"x-zp-page-request-id": "70687cf14221f34879a8999902443982-1548293302017-179605"}
        self.s.headers.update({'Host': 'fe-api.zhaopin.com',
                             'Origin': 'https://jobs.zhaopin.com',
                             'Referer': f'https://jobs.zhaopin.com/{jobNumber}.htm'})
        payload=f"""{{"jobNumbers":["{jobNumber}"],"cityIds":["763"],"resumeNumber":"{resumeNumber}","at":"c78e5dc3487f44dea5cf59afbec35dae","rt":"c0d0ef9c111c40d4a0b955976a033df1","language":1,"batched":false,"pageCode":4020,"jobSource":"SEARCH"}}"""

        r=self.s.post(url=url,params=params,data=payload)
        return r.json()

    def updateResume(self):
        # 可用来实现每天自动更新简历
        url="https://fe-api.zhaopin.com/c/i/resume/node"
        params={'at': 'c78e5dc3487f44dea5cf59afbec35dae',
                 'rt': 'c0d0ef9c111c40d4a0b955976a033df1',
                 '_v': '0.13205817',
                 'x-zp-page-request-id': '041588a1707e4697920a866c68ed0a96-1548294592462-763356'}
        self.s.headers.update({'Host': 'fe-api.zhaopin.com',
                             'Origin': 'https://i.zhaopin.com',
                             'Referer': 'https://i.zhaopin.com/resume'})
        payload="""your resume payload"""
        # encode是因为payload里面会出现中文
        r=self.s.post(url=url,params=params,data=payload.encode("utf-8"))
        return r.json()
```
