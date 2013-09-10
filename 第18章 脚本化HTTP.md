# 第18章 脚本化HTTP

## 18.1 使用XMLHttpRequest

    var request = new XMLHttpRequest()

一个HTTP请求由4部分组成：

* HTTP请求方法或“动作”（verb）
* 正在请求的URL
* 一个可选的请求头集合，其中可能包含身份验证信息
* 一个可选的请求主体

服务器返回的HTTP响应包含3部分：

* 一个数字和文字组成的状态码，用来显示请求的成功或失败
* 一个响应头集合
* 响应主体

### 18.1.1 指定请求

创建XMLHttpRequest对象之后，发起HTTP请求的下一步是调用XMLHttpRequest对象的open()方法去指定这个请求的两个必需部分：
<b>方法和URL</b>。

    request.open("GET","data.cvs")

open的第一个参数指定HTTP方法或动作。这个字符串不区分大小写，但通常大家用大写字母来匹配HTTP协议。“GET”用于常规的请求，它
适用于当URL完全指定请求资源，当请求对服务器没有任何副作用以及当服务器的响应是可缓存时。“POST”方法常用于HTML表单。它在请求
主体中包含额外数据（表单数据）且这些数据常存储到服务器上的数据库中（副作用）。相同的URL的重复POST请求从服务器得到的响应
可能不同，同时不应该缓存使用这个方法的请求。

如果有请求头的话，请求进程的下个步骤是设置它。例如，POST请求需要“Content-Type”头指定请求主体的MIME类型：

    request.setRequestHeader("Content-Type", "text/plain");

使用XMLHttpRequest发起HTTP请求的最后一步是指定可选的请求主体并向服务器发送它。使用send()方法像如下这样做：

    request.send(null);

GET请求绝对没有请求主体，所以应该传递null或省略这个参数。POST请求通常拥有主体，同时它应该匹配使用setRequestHeader()
指定的"Content-Type"头。

### 18.1.2 取得响应

一个完整的HTTP响应由状态码、响应头集合和响应主体组成。这些都可以通过XMLHttpRequest对象的属性和方法使用：

* status和statusText属性以数字和文本的形式返回HTTP状态码。这些属性保存标准的HTTP值，像200和“OK”表示成功请求，404和
“Not Found”表示URL不能匹配服务器上的任何资源。
* 使用getResponseHeader("KEY")和getAllResponseHeaders()能查询响应头。XMLHttpRequest会自动处理cookie：它会从
getAllResponseHeaders()头返回集合中过滤掉cookie头，而如果给getResponseHeader()传递“Set-Cookie”和“Set-Cookie2”则返回null。
* 响应主体可以从responseText属性中得到文本形式的，从responseXML属性中得到Document形式的。

### 18.1.3 编码请求主体

HTTP POST请求包含一个请求主体，它包含客户端传递给服务器的数据。

#### 1.表单编码的请求

考虑HTML表单。当用户提交表单时，表单中的数据编码到一个字符串中并随请求发送。默认情况下，HTML表单通过POST方法发送给服务器，
而编码后的表单数据则用做请求主体。对表单数据使用的编码方案相对简单：对每个表单元素的名字和值执行普通的URL编码（使用
十六进制转义码替换特殊字符），使用等号把编码后的名字和值分开，并使用“&”符号分开名/值对。

#### 2. JSON编码的请求

近年来，作为Web交换格式的JSON已经得到普及。例如：

    var request = new XMLHttpRequest();
    request.open("POST", '/login');
    request.onreadystatechange = function() {
        if(request.readyState === 4) {
            console.log(request);
        }
    }
    request.setRequestHeader("Content-Type", "application/json");
    request.send(JSON.stringify({"username":"zhangsan","password":"123456"}));

#### 3. XML编码的请求

XML有时也用于数据传输的编码。

#### 4. 上传文件

#### 5. multipart/form-data请求

当HTML表单同时包含文件上传元素和其他元素时，浏览器不能使用普通的表单编码而必须使用称为“multipart/form-data”的特殊
Content-Type来用POST方法提交表单。这种编码包括使用长“边界”字符串把请求主体分离成多个部分。对于文本数据，手动创建
“multipart/form-data”请求主体是可能的，但很复杂。

### 18.1.4 HTTP进度事件

HTTP请求无法完成有三种情况，对应三种事件：如果请求超时，会触发timeout事件。如果请求中止，会触发abort事件。最后，像太多
重定向这样的网络错误会阻止请求完成，但这些情况发生时会触发error事件。

### 18.1.5 中止请求和超时

作为同源策略的一部分，XMLHttpRequest对象通常仅可以发起和文档有相同服务器的HTTP请求。这个限制关闭了安全漏洞，但它笨手笨脚
并且也阻止了大量合适使用的跨域请求。



























