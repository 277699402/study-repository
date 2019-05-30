##### body-parser是什么?

    body-parser是一个HTTP请求体解析中间件，使用这个模块可以解析JSON、Raw、文本、URL-encoded格式的请求体，Express框架中就是使用这个模块做为请求体解析中间件。

    var express = require('express')
    //获取模块
    var bodyParser = require('body-parser')

    var app = express()

    // 创建 application/json 解析
    var jsonParser = bodyParser.json()

    // 创建 application/x-www-form-urlencoded 解析
    var urlencodedParser = bodyParser.urlencoded({ extended: false })

    // POST /login 获取 URL编码的请求体
    app.post('/login', urlencodedParser, function (req, res) {
      if (!req.body) return res.sendStatus(400)
      res.send('welcome, ' + req.body.username)
    })

    // POST /api/users 获取 JSON 编码的请求体
    app.post('/api/users', jsonParser, function (req, res) {
      if (!req.body) return res.sendStatus(400)
      // create user in req.body
    });
    app.listen(3000);

##### API

    对请求体的四种解析方式:

    1. bodyParser.json(options): 解析json数据
    2. bodyParser.raw(options): 解析二进制格式(Buffer流数据)
    3. bodyParser.text(options): 解析文本数据
    4. bodyParser.urlencoded(options): 解析UTF-8的编码的数据。

**bodyParser.json(options)**

    option可选对象:

    1. inflate - 设置为true时，deflate压缩数据会被解压缩；设置为true时，deflate压缩数据会被拒绝。默认为true。
    2. limit - 设置请求的最大数据量。默认为'100kb'
    3. reviver - 传递给JSON.parse()方法的第二个参数，详见JSON.parse()
    4. strict - 设置为true时，仅会解析Array和Object两种格式；设置为false会解析所有JSON.parse支持的格式。默认为true
    5. type - 该选项用于设置为指定MIME类型的数据使用当前解析中间件。这个选项可以是一个函数或是字符串，当是字符串是会使用type-is来查找MIMI类型；当为函数是，中间件会通过fn(req)来获取实际值。默认为application/json。
    6. verify - 这个选项仅在verify(req, res, buf, encoding)时受支持

**bodyParser.raw(options)**

    返回一个将所有数据做为**Buffer格式**处理的中间件.其后的所有的req.body中将会是一个Buffer值。

    option可选值:

    1. inflate - 设置为true时，deflate压缩数据会被解压缩；设置为true时，deflate压缩数据会被拒绝。默认为true。
    2. limit - 设置请求的最大数据量。默认为'100kb'
    3. type - 该选项用于设置为指定MIME类型的数据使用当前解析中间件。这个选项可以是一个函数或是字符串，当是字符串是会使用type-is来查找MIMI类型；当为函数是，中间件会通过fn(req)来获取实际值。默认为application/octet-stream。
    4. verify - 这个选项仅在verify(req, res, buf, encoding)时受支持

**bodyParser.text(options) 解析文本格式**

    返回一个仅处理字符串格式处理的中间件。其后的所有的req.body中将会是一个字符串值。

    1. defaultCharset - 如果Content-Type后没有指定编码时，使用此编码。默认为'utf-8'
    2. inflate - 设置为true时，deflate压缩数据会被解压缩；设置为true时，deflate压缩数据会被拒绝。默认为true。
    3. limit - 设置请求的最大数据量。默认为'100kb'
    4. type - 该选项用于设置为指定MIME类型的数据使用当前解析中间件。这个选项可以是一个函数或是字符串，当是字符串是会使用type-is来查找MIMI类型；当为函数是，中间件会通过fn(req)来获取实际值。默认为application/octet-stream。
    5. verify - 这个选项仅在verify(req, res, buf, encoding)时受支持

**bodyParser.urlencoded(options) 解析UTF-8的编码的数据.返回一个处理urlencoded数据的中间件.**

    option可选值

    1. extended - 当设置为false时，会使用querystring库解析URL编码的数据；当设置为true时，会使用qs库解析URL编码的数据。后没有指定编码时，使用此编码。默认为true
    2. inflate - 设置为true时，deflate压缩数据会被解压缩；设置为true时，deflate压缩数据会被拒绝。默认为true。
    3. limit - 设置请求的最大数据量。默认为'100kb'
    4. parameterLimit - 用于设置URL编码值的最大数据。默认为1000
    5. type - 该选项用于设置为指定MIME类型的数据使用当前解析中间件。这个选项可以是一个函数或是字符串，当是字符串是会使用type-is来查找MIMI类型；当为函数是，中间件会通过fn(req)来获取实际值。默认为application/octet-stream。
    6. verify - 这个选项仅在verify(req, res, buf, encoding)时受支持

    代码示例:

    var express = require('express')
    var bodyParser = require('body-parser')
    var app = express()

    // create application/json parser
    var jsonParser = bodyParser.json()
    // create application/x-www-form-urlencoded parser
    var urlencodedParser = bodyParser.urlencoded({ extended: false })

    // POST /home 获取 urlencoded bodies
    app.post('/home', urlencodedParser, function (req, res) {
      if (!req.body) return res.sendStatus(400)
      res.send('welcome, ' + req.body.username)
    })

    // POST /api/users 获取 JSON bodies
    app.post('/about', jsonParser, function (req, res) {
      if (!req.body) return res.sendStatus(400)
      res.send('welcome ****, ' + req.body.username)
    });

    app.listen(3000);