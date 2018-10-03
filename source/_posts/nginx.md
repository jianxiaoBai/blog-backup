---
title:  nginx 笔记
date: 2018-03-04 20:35:57
categories: [技术笔记]
tags: [nginx, linux, mac, 笔记]
comments: false
---

![](../imgs/nginx-proxy.jpg)

<!-- more -->

[图源来自知乎](https://www.zhihu.com/question/24723688/answer/48369770)

## 反向代理的优点

- `保护`真实 web 服务器，其对外不可见；
- `节约`了有限的 ip 地址资源；
- `减少` web 服务器压力，提高响应速度；
- 请求的统一控制，包括设置权限、过滤规则等；
- 区分动态和静态可缓存内容；
- 实现负载均衡，内部可以采用多台服务器来组成服务器集群，外部还是可以采用一个地址访问；
- 解决 Ajax 跨域问题；
- 作为真实服务器的缓冲，解决瞬间负载量大的问题；

## 配置文件

```
main # 全局设置
events { # Nginx工作模式
    ....
}
http { # http设置
    ....
    upstream myproject { # 负载均衡服务器设置
        .....
    }
    server  { # 主机设置
        ....
        location { # URL匹配
            ....
        }
    }
    server  {
        ....
        location {
            ....
        }
    }
    ....
}

```

## 功能实现

### 基于域名的虚拟主机

第一步: 假设我们在本地开发有3个项目，需要分别在 hosts 里`映射到本地`的127.0.0.1上：

```
127.0.0.1 www.aaa.com
127.0.0.1 www.bbb.com
127.0.0.1 www.ccc.com
```

第二步: 分别对应于web根目录下的3个文件夹，用域名`对应文件夹`名字，为了好记：

```
/Users/baixiaojian/www/www.aaa.com/
/Users/baixiaojian/www/www.bbb.com/
/Users/baixiaojian/www/www.ccc.com/
```

每个目录都有一个 index.html 文件, 都是简单的输出自己的域名.

第三步: 我们要新建3个server来`搭建对应个域名的虚拟主机`。

> 这3个 server 配置信息都写成一个 .conf 的配置文件,然后在 http 模块统一导入,这样比较便于维护和管理

```
main
events {
    ....
}
http {
    ....
    include servers/www.aaa.conf;
    include servers/www.bbb.conf;
    include servers/www.ccc.conf;
    # 或者用 *.conf  包含
    # include servers/*.conf
}
```

既然每一个conf都是一个server，下面就开始：

```
### www.aaa.com
  server {
      charset utf-8;

      listen       80;
      server_name  www.aaa.com;

      location / {
          root   /Users/baixiaojian/www/www.aaa.com/;
          index  index.html index.htm;
      }
  }
```


```
### www.bbb.com
  server {
      charset utf-8;

      listen       80;
      server_name  www.bbb.com;

      location / {
          root   /Users/baixiaojian/www/www.bbb.com/;
          index  index.html index.htm;
      }
  }
```


```
### www.ccc.com
  server {
      charset utf-8;

      listen       80;
      server_name  www.ccc.com;

      location / {
          root   /Users/baixiaojian/www/www.ccc.com/;
          index  index.html index.htm;
      }
  }
```

这样3个很精简的虚拟域名就搭建好了。
重启下nginx，然后打开浏览器访问一下这3个域名，就能看到对应的域名内容了。

### 反向代理

**Nginx 使用反向代理，主要是使用location模块下的 `proxy_pass` 选项。**

```
来个最简单的。
当我访问 mac 上的nginx 的 centos.com 的内容时候,
就反向代理到虚拟机centos上的 10.211.55.5 的index.html页面。
```

第一步: 在 hosts 里新加域名:

```
#vi /etc/hosts
127.0.0.1 centos.com
```

第二步: 在 servers 目录中新建一个 .conf 的配置文件：

```
#centos.conf
server {
    listen 80;
    server_name centos.com;

    location / {
        proxy_pass http://10.211.55.5;
    }
}
```

执行 `sudo nginx -s reload` 重启后访问 `centos.com`

### 负载均衡

啥是负载均衡 ?
比如我们有一个小网站，刚开始就一台nginx服务器，
后来，随着业务量增大，用户增多，一台服务器已经不够用了，
我们就又多加了几台服务器。那么这几台服务器 `如何调度` ？`如何均匀提供访问` ？
这就是负载均衡。

**负载均衡的好处是可以集群多台机器一起工作，并且对外的IP和域名是一样的，外界看起来就好像一台机器一样。**

#### 基于 weight 权重的负载

```
upstream webservers{
    server 192.168.33.11 weight=10;
    server 192.168.33.12 weight=10;
    server 192.168.33.13 weight=10;
}

server {
    listen 80;
    server_name upstream.com;

    location / {
        proxy_pass http://webservers;
        proxy_set_header  X-Real-IP  $remote_addr;
    }
}
```

#### 基于 ip_hash 的负载

**每个请求按访问IP的hash结果分配**，这样来自同一个IP的访客固定访问一个后端服务器，有效解决了动态网页存在的session共享问题。

```
upstream webservers{
    ip_hash;
    server 192.168.33.11 weight=1 max_fails=2 fail_timeout=30s;
    server 192.168.33.12 weight=1 max_fails=2 fail_timeout=30s;
    server 192.168.33.13 down;
}
```

注: ip_hash 模式下，不要设置 `weight` 和 `backup`

### 页面缓存

只需要简单配置下，就能将指定的一个页面缓存起来.
原理就是`匹配当前访问的url, hash加密后，去指定的缓存目录查找`, 有的话就说明匹配到缓存.

**先来看一下一个简单的页面缓存的配置：**

```
http {
    proxy_cache_path /data/nginx/cache levels=1:2 keys_zone=cache_zone:10m inactive=1d max_size=100m;
    # proxy_cache_path path [levels=number] keys_zone=zone_name:zone_size [inactive=time] [max_size=size];
                                                    # path        => 缓存路径
                                                    # levels      => 文件夹级数
                                                    # keys_zone   => 指的是共享池的名称
                                                    # inactive    => 表示指定的时间内缓存的数据没有被请求则被删除
                                                    # max_size    => 缓存区域的总大小
                                                    # clean_time  => 表示每间隔自动清除的时间
    upstream myproject {
        .....
    }
    server  {
        ....
        location ~ *\.php$ {
            proxy_cache cache_zone;                 # keys_zone的名字
            proxy_cache_key $host$uri$is_args$args; # 缓存规则
            proxy_cache_valid any 1d;               # 为不同的http响应状态码设置不同的缓存时间。
            proxy_pass http://127.0.0.1:8080;
        }
    }
    ....
}
```

**开始进行实战**

第一步: Parallels Desktop 上启动一台 linux 虚拟机(10.211.55.5)

第二部: Mac hosts配置 `cache.com` 域名, 然后按照上面的配置在 servers 下新建一个 `cache.conf` 文件:

```
proxy_cache_path /usr/local/var/cache levels=1:2 keys_zone=cache_zone:10m inactive=1d max_size=100m;
server  {
    listen 80;
    server_name cache.com;

    add_header X-Via $server_addr;
    add_header X-Cache $upstream_cache_status;

    location / {
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_cache cache_zone;
        proxy_cache_key $host$uri$is_args$args;
        proxy_cache_valid 200 304 1m;
        proxy_pass http://10.211.55.5;
    }
}
```

访问 `cache.com` 查看 `network` 网络请求选项，我们可以看到，Response Headers，在这里我们可以看到：

```
X-Cache: MISS
X-Via:  127.0.0.1
```

X-cache 为 MISS 表示未命中，请求被传送到后端。
因为是第一次访问，没有缓存，所以肯定是未命中。
我们再刷新下，就发现其变成了HIT, 表示命中。

X-cache 的其他几种状态：

| 状态     | 说明                         |
| -------- | ---------------------------- |
| MISS     | 未命中，请求被传送到后端     |
| HIT      | 缓存命中                     |
| EXPIRED  | 缓存已经过期请求被传送到后端 |
| UPDATING | 正在更新缓存，将使用旧的应答 |
| STALE    | 后端将得到过期的应答         |
| BYPASS   | 缓存被绕过了                 |

我们再去看看缓存文件夹 /usr/local/var/cache里面是否有了文件：

```
cache git:(master) cd a/13
➜  13 git:(master) ls
5bd1af99bcb0db45c8bd601d9ee9e13a
➜  13 git:(master) pwd
/usr/local/var/cache/a/13
```

已经生成了缓存文件。

我们在url 后面随便加一个什么参数，
看会不会新生成一个缓存文件夹及文件：http://cache.com/?w=ww55 。
因为我们使用的生成规则是全部url转换(proxy_cache_key $host$uri$is_args$args;)

查看 X-cache 为 MISS，再刷新 ，变成HIT。
缓存文件夹 /usr/local/var/cache 又会多出对应缓存文件.

### Location 正则模块

`location /` 表示匹配访问根目录。
`location ~` 表示开启正则匹配。
还可以用这个来匹配静态资源，缓存它们，设置过期时间：

```
location ~ .*\.(gif|jpg|jpeg|bmp|png|ico|txt|mp3|mp4|swf){
    expires 15d;
}
location ~ .*\.(css|js){
    expires 12d;
}
```

-----------

## 配置说明

```
# 运行用户
user www-data;
# 启动进程,通常设置成和cpu的数量相等
worker_processes  1;

# 全局错误日志及PID文件
error_log  /var/log/nginx/error.log;
pid        /var/run/nginx.pid;

# 工作模式及连接数上限
events {
    use epoll; #epoll是多路复用IO(I/O Multiplexing)中的一种方式,但是仅用于linux2.6以上内核,可以大大提高nginx的性能
    worker_connections 1024; #单个后台worker process进程的最大并发链接数
    # multi_accept on;
}

#设定http服务器，利用它的反向代理功能提供负载均衡支持
http {
    #设定mime类型,类型由mime.type文件定义
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    #设定日志格式
    access_log    /var/log/nginx/access.log;

    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，对于普通应用，
    #必须设为 on,如果用来进行下载等应用磁盘IO重负载应用，可设置为 off，以平衡磁盘与网络I/O处理速度，降低系统的uptime.
    sendfile        on;
    #将tcp_nopush和tcp_nodelay两个指令设置为on用于防止网络阻塞
    tcp_nopush      on;
    tcp_nodelay     on;
    #连接超时时间
    keepalive_timeout  65;

    #开启gzip压缩
    gzip  on;
    gzip_disable "MSIE [1-6]\.(?!.*SV1)";

    #设定请求缓冲
    client_header_buffer_size    1k;
    large_client_header_buffers  4 4k;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;

    #设定负载均衡的服务器列表
    upstream mysvr {
        #weigth参数表示权值，权值越高被分配到的几率越大
        #本机上的Squid开启3128端口
        server 192.168.8.1:3128 weight=5;
        server 192.168.8.2:80  weight=1;
        server 192.168.8.3:80  weight=6;
    }


    server {
        #侦听80端口
        listen       80;
        #定义使用www.xx.com访问
        server_name  www.xx.com;

        #设定本虚拟主机的访问日志
        access_log  logs/www.xx.com.access.log  main;

        #默认请求
        location / {
            root   /root;                               #定义服务器的默认网站根目录位置
            index index.php index.html index.htm;       #定义首页索引文件的名称

            fastcgi_pass  www.xx.com;
            fastcgi_param  SCRIPT_FILENAME  $document_root/$fastcgi_script_name;
            include /etc/nginx/fastcgi_params;
        }

        # 定义错误提示页面
        error_page   500 502 503 504 /50x.html;
            location = /50x.html {
            root   /root;
        }

        #静态文件，nginx自己处理
        location ~ ^/(images|javascript|js|css|flash|media|static)/ {
            root /var/www/virtual/htdocs;
            #过期30天，静态文件不怎么更新，过期可以设大一点，如果频繁更新，则可以设置得小一点。
            expires 30d;
        }
        #PHP 脚本请求全部转发到 FastCGI处理. 使用FastCGI默认配置.
        location ~ \.php$ {
            root /root;
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME /home/www/www$fastcgi_script_name;
            include fastcgi_params;
        }
        #设定查看Nginx状态的地址
        location /NginxStatus {
            stub_status            on;
            access_log              on;
            auth_basic              "NginxStatus";
            auth_basic_user_file  conf/htpasswd;
        }
        #禁止访问 .htxxx 文件
        location ~ /\.ht {
            deny all;
        }
    }

    #第一个虚拟服务器
    server {
        #侦听192.168.8.x的80端口
        listen       80;
        server_name  192.168.8.x;

        #对aspx后缀的进行负载均衡请求
        location ~ .*\.aspx$ {
            root   /root;#定义服务器的默认网站根目录位置
            index index.php index.html index.htm;#定义首页索引文件的名称

            proxy_pass  http://mysvr;#请求转向mysvr 定义的服务器列表

            #以下是一些反向代理的配置可删除.
            proxy_redirect off;

            #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            client_max_body_size 10m;                                       #允许客户端请求的最大单文件字节数
            client_body_buffer_size 128k;                                   #缓冲区代理缓冲用户端请求的最大字节数，
            proxy_connect_timeout 90;                                       #nginx跟后端服务器连接超时时间(代理连接超时)
            proxy_send_timeout 90;                                          #后端服务器数据回传时间(代理发送超时)
            proxy_read_timeout 90;                                          #连接成功后，后端服务器响应时间(代理接收超时)
            proxy_buffer_size 4k;                                           #设置代理服务器（nginx）保存用户头信息的缓冲区大小
            proxy_buffers 4 32k;                                            #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
            proxy_busy_buffers_size 64k;                                    #高负荷下缓冲大小（proxy_buffers*2）
            proxy_temp_file_write_size 64k;                                 #设定缓存文件夹大小，大于这个值，将从upstream服务器传
        }
    }
}
```

[参考资料](https://www.jianshu.com/p/bed000e1830b)