##### Nginx 反向代理与负载均衡

## proxy_pass

    nginx反向代理主要通过proxy_pass来配置，将你项目的开发机地址填写到proxy_pass后面，正常的格式为proxy_pass URL即可

    server {
        listen 80;
        location / {
            proxy_pass http://10.10.10.10:20186;
        }
    }

## Upstream模块实现负载均衡

    // 修改nginx.conf
    worker_processes 1;
    events {
        worker_connections 1024;
    }
    http {
        upstream firstdemo {
            server 39.106.145.33;
            server 47.93.6.93;
        }
        server {
            listen 8080;
            location / {
                proxy_pass http://firstdemo;
            }
        }
    }

    worker_processes    工作进程数，和CPU核数相同
    worker_connections  每个进程允许的最大连接数
    upstream模块         负载均衡就靠它  语法格式：upstream name {}

    里面写的两个server分别对应着不同的服务器
    server模块    实现反向代理
    listen        监督端口号
    location / {}访问根路径
    proxy_pass http://firstdemo，代理到firstdemo里两个服务器上

## ip_hash

    当用户第一次访问到其中一台服务器后，下次再访问的时候就直接访问该台服务器

    upstream firstdemo {
        ip_hash;
        server 39.106.145.33;
        server 47.93.6.93;
    }

## 例子

    server {
        listen       80;
        server_name  chd.news.so.m.qss.test.so.com ;
        auth_basic off;
        location / {
            proxy_pass    http://10.10.10.10:20186;
            proxy_set_header Host $host;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_connect_timeout 60;
            proxy_read_timeout 600;
            proxy_send_timeout 600;
        }
    }


