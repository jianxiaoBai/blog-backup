---
title: mongodb 安装与基本配置
date: 2018-01-26 15:24:36
tags: [mongod, 基本配置]
categories: [配置相关]
comments: false
---

## 查看

brew list
brew search mongodb

## 安装

brew install mongodb

## 启动

首次执行 `mongod` 尝试启动 MongoDB , 但会失败 exiting:

<!-- more -->

```
➜  ~  mongod
2018-01-26T15:11:13.278+0800 I CONTROL  [initandlisten] MongoDB starting : pid=8954 port=27017 dbpath=/data/db 64-bit host=baixiaojiandeMacBook-Pro.local
2018-01-26T15:11:13.278+0800 I CONTROL  [initandlisten] db version v3.6.2
2018-01-26T15:11:13.278+0800 I CONTROL  [initandlisten] git version: 489d177dbd0f0420a8ca04d39fd78d0a2c539420
2018-01-26T15:11:13.278+0800 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2n  7 Dec 2017
2018-01-26T15:11:13.278+0800 I CONTROL  [initandlisten] allocator: system
2018-01-26T15:11:13.278+0800 I CONTROL  [initandlisten] modules: none
2018-01-26T15:11:13.278+0800 I CONTROL  [initandlisten] build environment:
2018-01-26T15:11:13.278+0800 I CONTROL  [initandlisten]     distarch: x86_64
2018-01-26T15:11:13.278+0800 I CONTROL  [initandlisten]     target_arch: x86_64
2018-01-26T15:11:13.278+0800 I CONTROL  [initandlisten] options: {}
2018-01-26T15:11:13.279+0800 I STORAGE  [initandlisten] exception in initAndListen: NonExistentPath: Data directory /data/db not found., terminating
2018-01-26T15:11:13.279+0800 I CONTROL  [initandlisten] now exiting
2018-01-26T15:11:13.279+0800 I CONTROL  [initandlisten] shutting down with code:100
```

启动 MongoDB 之前, 要先新建一个 MongoDB 默认的数据写入目录

```
// sudo 并输入密码，重新新建目录
$ sudo mkdir -p /data/db
Password:
```

给刚才新建的数据库目录赋予权限:

```
$ sudo chown -R baixiaojian /data
```

此时,执行 `mongod` 启动 MongoDB 服务:

```
➜  ~ mongod
2018-01-26T15:17:54.988+0800 I CONTROL  [initandlisten] MongoDB starting : pid=9551 port=27017 dbpath=/data/db 64-bit host=baixiaojiandeMacBook-Pro.local
2018-01-26T15:17:54.989+0800 I CONTROL  [initandlisten] db version v3.6.2
2018-01-26T15:17:54.989+0800 I CONTROL  [initandlisten] git version: 489d177dbd0f0420a8ca04d39fd78d0a2c539420
2018-01-26T15:17:54.989+0800 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2n  7 Dec 2017
2018-01-26T15:17:54.989+0800 I CONTROL  [initandlisten] allocator: system
2018-01-26T15:17:54.989+0800 I CONTROL  [initandlisten] modules: none
2018-01-26T15:17:54.989+0800 I CONTROL  [initandlisten] build environment:
2018-01-26T15:17:54.989+0800 I CONTROL  [initandlisten]     distarch: x86_64
2018-01-26T15:17:54.989+0800 I CONTROL  [initandlisten]     target_arch: x86_64
2018-01-26T15:17:54.989+0800 I CONTROL  [initandlisten] options: {}
2018-01-26T15:17:54.990+0800 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=7680M,session_max=20000,eviction=(threads_min=4,threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),statistics_log=(wait=0),verbose=(recovery_progress),
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten]
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten]
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] ** WARNING: This server is bound to localhost.
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          Remote systems will be unable to connect to this server.
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          Start the server with --bind_ip <address> to specify which IP
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          addresses it should serve responses from, or with --bind_ip_all to
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          bind to all interfaces. If this behavior is desired, start the
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          server with --bind_ip 127.0.0.1 to disable this warning.
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten]
2018-01-26T15:17:55.360+0800 I STORAGE  [initandlisten] createCollection: admin.system.version with provided UUID: acffed24-1106-4136-bf4d-d2894c0cb3e3
2018-01-26T15:17:55.443+0800 I COMMAND  [initandlisten] setting featureCompatibilityVersion to 3.6
2018-01-26T15:17:55.447+0800 I STORAGE  [initandlisten] createCollection: local.startup_log with generated UUID: 5414b600-b049-49b5-aad7-c20beb480c41
2018-01-26T15:17:55.551+0800 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/db/diagnostic.data'
2018-01-26T15:17:55.551+0800 I NETWORK  [initandlisten] waiting for connections on port 27017
```

MongoDB 启动成功,等待被连接中...

## 连接

新建命令行窗口, 执行 `mongo` 进入 MongoDB 命令行模式: 

```
➜  ~ mongo
MongoDB shell version v3.6.2
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.6.2
Server has startup warnings:
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten]
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten]
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] ** WARNING: This server is bound to localhost.
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          Remote systems will be unable to connect to this server.
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          Start the server with --bind_ip <address> to specify which IP
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          addresses it should serve responses from, or with --bind_ip_all to
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          bind to all interfaces. If this behavior is desired, start the
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten] **          server with --bind_ip 127.0.0.1 to disable this warning.
2018-01-26T15:17:55.353+0800 I CONTROL  [initandlisten]
> 4685*989
4633465
>

```

连接成功, 然后就可以对数据库进行操作了.