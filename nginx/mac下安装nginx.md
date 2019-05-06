# 步骤

## 一、打开终端，习惯性命令

    brew update
    //结果：Already up-to-date.

## 二、终端继续执行命令

    brew search nginx   //查询要安装的软件是否存在

## 三、这里我们多执行查看详情命令，有利于我们后面的配置

    brew info nginx

    运行结果:

![图片](/nginx/images/nginx1.jpg)

    我们可以看到，nginx在本地还未安装（Not installed），nginx的来源（From），Docroot默认为/usr/local/var/www，在/usr/local/etc/nginx/nginx.conf配置文件中默认端口被配置为8080从而使nginx运行时不需要加sudo，nginx将在/usr/local/etc/nginx/servers/目录中加载所有文件，以及我们可以通过最简单的命令 ‘nginx’ 来启动nginx。

## 四、正式开始安装

    brew install nginx

![图片](/nginx/images/nginx2.jpg)

## 五、查看nginx安装目录（是否如info所说）

    open /usr/local/etc/nginx/

    成功打开nginx目录，也可以看到如info所说servers目录以及nginx.conf的配置文件（后面会用到这个配置文件）。但我们并没有找到nginx被安装到了哪里。

    终端继续执行:

    open /usr/local/Cellar/nginx  //其实这个才是nginx被安装到的目录

## 六、启动nginx，终端输入如下命令

    nginx

## 七、访问验证:

  打开浏览器访问localhost:8080






