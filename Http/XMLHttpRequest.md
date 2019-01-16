# Selenium 中使用 XMLHttpRequest

## 一、XMLHttpRequest的基本知识

[学习资料：MDN Web技术文档](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)

1. Constructor

    ```javascript
    const xhr=new XMLHttpRequest();//创建一个XMLHttpRequest对象，必须在调用其他方法前调用。
    ```

1. Methods

    * open

        ```javascript
        xhr.open(method, url, async, user, password);//初始化XMLHttpRequest对象
        ```

    * setRequestHeader

        ```javascript
        xhr.setRequestHeader(header, value);//设置Http Request Header,必须在open()之后，send()之前调用
        ```

    * overrideMimeType

        ```javascript
        xhr.overrideMimeType(mimeType);//重写服务器返回的数据为指定类型，必须在send()之前调用。使用responseType修改更方便，见下文
        ```

    * send

        ```javascript
        xhr.send(body);//发送请求到服务器
        ```

    * getAllResponseHeaders

        ```javascript
        const headers=xhr.getAllResponseHeaders();//返回所有的响应头部
        ```

    * getResponseHeader

        ```javascript
        const header=xhr.getRsponseHeader(headerName);//返回指定响应头部
        ```

    * abort

        ```javascript
        xhr.abort();//终止请求，在send()之后调用
        ```

1. Properties

    * readyState

        ```javascript
        const state=xhr.readyState;// 4
        ```
        表示XMLHttpRequest对象的状态，一共有以下5种状态。

        Value | State | Description
        ------|-------|------------
        0 | UNSENT | Client has been created. open() not called yet.
        1 | OPENED | open() has been called.
        2 | HEADERS_RECEIVED | send() has been called, and headers and status are available.
        3 | LOADING | Downloading; responseText holds partial data.
        4 | DONE | The operation is complete.

    * onreadystatechange

        ```javascript
        xhr.onreadystatechange=callback;//注意，callback是一个函数，在readyState改变的时候执行。
        ```

    * status,statusText

        ```javascript
        //表示XMLHttpRequest响应的状态,status返回数值，statusText返回文本。在XMLHttpRequest完成前或者报错时，会返回 status=0 。
        const status=xhr.status;// 200
        const status_text=xhr.statusText;// "OK"
        ```
    * response,responseText,responseXML

        ```javascript
        //表示XMLHttpRequest响应的正文。
        const rep=xhr.response;
        ```
    * responseURL

        ```javascript
        //表示响应的URL，如果多次重定向，则返回最终的URL。
        const url=xhr.responseURL;
        ```
    * responseType

        ```javascript
        //表示响应的数据类型，可以手动修改为指定类型。
        var type=xhr.responseType;//"",空字符串表示“text”
        xhr.responseType="json";//修改为json格式
        ```
    * timeout

        ```javascript
        xhr.timeout=2000;//request超时时间设置为2000ms
        ```
    * upload
        返回一个 XMLHttpRequestUpload对象，用来表示上传的进度。

    * withCredentials

        ```javascript
        xhr.withCredentials = true;//指示是否该使用类似cookies,authorization headers(头部授权)或者TLS客户端证书这一类资格证书来创建一个跨站点访问控制（cross-site Access-Control）请求。在同一个站点下使用withCredentials属性是无效的。
        ```

1. Events

    Event | 触发条件
    ---------|----------|
    loadstart | 开始加载资源 |
    progress | 操作进行中
    readystatechange|XMLHttpRequest的readyState改变
    load|资源加载完毕
    abort | 中止加载资源后
    error|资源加载失败
    loadend|资源加载已终止，可能是load,abort,error之一触发
    timeout|超时

1. 结合使用

    ```javascript
    //GET方法
    var xhr = new XMLHttpRequest()；
    xhr.open("GET", "https://developer.mozilla.org/", true);
    xhr.onreadystatechange = function () {
    if(xhr.readyState === 4 && xhr.status === 200) {
        console.log(xhr.responseText);
    }
    };
    xhr.send();
    ```

    ```javascript
    //POST 方法
    var xhr = new XMLHttpRequest();
    xhr.open("POST", '/server', true);
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    xhr.onreadystatechange = function() {
        if (this.readyState === XMLHttpRequest.DONE && this.status === 200) {
            console.log(xhr.responseText)
        }
    }
    xhr.send("foo=bar&lorem=ipsum");
    ```

## 二、在Selenium中使用XMLHttpRequest

1. 具体实现
    jupyter noebook脚本截图：
    ![XMLHttpRequest-jupyter截图](https://raw.githubusercontent.com/opwtryl/photos/master/XMLHttpRequest/xhr_jupyter_190112.png)

    Chrome开发者工具network截图：
    ![XHR-network](https://raw.githubusercontent.com/opwtryl/photos/master/XMLHttpRequest/xhr_network_190112.png)
    ![XHR-headers](https://raw.githubusercontent.com/opwtryl/photos/master/XMLHttpRequest/xhr_headers_190112.png)

    图2红框里的XMLHttpRequest就是通过图1里的脚本完成的。

    需要注意的是，selenium是通过匿名函数的形式执行javascript脚本的。即:

    ```python
    driver.execute_script("return result")
    ```

    实际执行的javascript脚本是：

    ```javascript
    (function(){ return result;}());
    ```

    所以为了等待XMLHttpRequest完成后再返回值，使用

    ```javascript
    result=[];//全局变量
    ```

    而不是

    ```javascript
    var result=[];//局部变量，使用driver.execute_script("return result")会报错：result not defined
    ```
