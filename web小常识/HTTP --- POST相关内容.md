### HTTP --- POST相关内容

    HTTP 协议是以 ASCII 码传输，建立在 TCP/IP 协议之上的应用层规范。规范把 HTTP 请求分为三个部分：状态行、请求头、消息主体。HTTP/1.1 协议规定的 HTTP 请求方法有 OPTIONS、GET、HEAD、POST、PUT、DELETE、TRACE、CONNECT 这几种。

**协议规定 POST 提交的数据必须放在消息主体（entity-body）中，但协议并没有规定数据必须使用什么编码方式。**

    POST 提交数据时，包含了 Content-Type 和消息主体编码方式两部分。因为服务器端通常会依据Content-Type来决定使用何种方式解析主体部分.

    四种方式:

    1. application/x-www-form-urlencoded:提交的数据按照 key1=val1&key2=val2 的方式进行编码，key 和 val 都进行了 URL 转码。
    2. multipart/form-data:使用表单上传文件时，必须让 <form> 表单的 enctype 等于 multipart/form-data
    3. application/json: 用来告诉服务端消息主体是序列化后的 JSON 字符串。
    4. text/xml: 它是一种使用 HTTP 作为传输协议，XML 作为编码方式的远程调用规范。

    其中application/x-www-form-urlencoded编码其实是基于uri的percent-encoding编码的，所以采用application/x-www-form-urlencoded的POST数据和queryString只是形式不同，本质都是传递参数。