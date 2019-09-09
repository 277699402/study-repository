# 查看网页源码
## curl -o  保存页面,相当于wget命令

    命令名称：curl -o [文件名] www.sina.com

## curl -L  自动跳转

    命令名称：curl -L www.sina.com

## curl -i  显示头信息

    命令名称：curl -i www.sina.com

    '-I' 参数则是只显示http response 信息

## curl -v 显示通信过程

    命令名称：curl -v www.sina.com

## 发送表单信息

    发送GET： curl example.com/form.cgi?data=xxx

    发送POST：curl -X POST --data "data=xxx" example.com/form.cgi

    如果你的数据没有经过表单编码，还可以让curl为你编码，参数是`--data-urlencode`。

    curl -X POST--data-urlencode "date=April 1" example.com/form.cgi

## 文件上传

    curl --form upload=@localfilename --form press=OK [URL]

## Referer字段

    curl --referer http://www.example.com http://www.example.com

## User Agent字段

    curl --user-agent "[User Agent]" [URL]

## cookie

    curl --cookie "name=xxx" www.example.com

    `-c cookie-file`可以保存服务器返回的cookie到文件，`-b cookie-file`可以使用这个文件作为cookie信息，进行后续的请求。

    curl -c cookies http://example.com
    curl -b cookies http://example.com

## 增加头信息

    curl --header "Content-Type:application/json" http://example.com

## HTTP认证

    curl --user name:password example.com












