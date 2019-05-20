# Selenium 反爬 资料汇总

## 修改chromedriver特征值

https://intoli.com/blog/not-possible-to-block-chrome-headless/

https://zhuanlan.zhihu.com/p/43581988

## 设置chromedriver选项

option.add_experimental_option('excludeSwitches', ['enable-automation'])

## 接管已打开的浏览器

```python

import os
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

os.system(r"chrome --remote-debugging-port=9223") #也可以通过系统的 CMD 打开，注意：需要把已打开的chrome浏览器全部关闭，或者指定另一个免安装版本的chrome浏览器

chromeOptions = Options()
chromeOptions.add_experimental_option("debuggerAddress", "127.0.0.1:9223")
driver = webdriver.Chrome(executable_path=r"F:\selenium\chromedriver.exe",options=chromeOptions)

```

## 修改chromedriver源代码

参考(windows也适用)：
https://stackoverflow.com/questions/33225947/can-a-website-detect-when-you-are-using-selenium-with-chromedriver
