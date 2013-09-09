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
* 




























