# blog-starter

## 启动

```bash
$ cd blog-starter
```

编辑 ```.env``` 文件，根据个人喜好修改环境变量。本地调试时，如果自定义域名，需要修改 ```/ect/hosts``` 文件，示例如下：

```
27.0.0.1 example.com
127.0.0.1 lychee.example.com
127.0.0.1 netdata.example.com
```

通过 docker 安装镜像并启动。

```bash
$ docker-compose up
```

## Lychee 图床配置

浏览器访问 http://lychee.example.com，首次访问会要求输入账号和密码，示例如下：

```
host: mysql # 固定值
user: root # 见 ${MYSQL_ROOT_USER}
password: root_password # 见 ${MYSQL_ROOT_PASSWORD}
```

## MySQL 配置

MySQL 的账号密码默认见 ```.env``` 文件。

## Wordpress 配置

登陆 MySQL，查看 Wordpress 对应的数据库，示例如下：

```bash
→ docker ps
CONTAINER ID   IMAGE                                    COMMAND                  CREATED             STATUS         PORTS                                      NAMES
abcc6faab14d   wordpress:php7.3                         "docker-entrypoint.s…"   4 minutes ago       Up 4 minutes   80/tcp                                     wordpress
5dd6869d8375   jrcs/letsencrypt-nginx-proxy-companion   "/bin/bash /app/entr…"   About an hour ago   Up 4 minutes                                              letsencrypt
4a1257042dd2   linuxserver/lychee:v3.2.16-ls35          "/init"                  About an hour ago   Up 4 minutes   80/tcp, 443/tcp                            lychee
06ab27a14975   mysql:5.6                                "docker-entrypoint.s…"   About an hour ago   Up 4 minutes   3306/tcp                                   mysql
71344c0cba56   jwilder/nginx-proxy                      "/app/docker-entrypo…"   About an hour ago   Up 4 minutes   0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   nginx-proxy
27512ba8299b   netdata/netdata:v1.18.1                  "/usr/sbin/run.sh"       About an hour ago   Up 4 minutes   19999/tcp                                  netdata
→ docker exec -it 06ab27a14975 /bin/sh
→ mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.6.51 MySQL Community Server (GPL)

Copyright (c) 2000, 2021, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.01 sec)
# create database if not exists `vanleon` default charset utf8mb4 collate utf8mb4_general_ci;
Query OK, 1 row affected (0.01 sec)
```

浏览器访问 http://example.com/wp-admin/install.php，准备初始化 Wordpress，配置完成后，页面会自动重定向到 http://example.com/wp-login.php 管理后台登陆页面，使用刚刚配置的账号和密码登陆即可。

访问 http://example.com 即可以看到通过 Wordpress 搭建的 Blog。
