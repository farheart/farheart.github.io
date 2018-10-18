---
layout: post
title: Reading Notes -- Docker架设Nginx服务器
category: Docker
tags: Docker  Web
keywords: Docker  Nginx
published: true
---

# Reading Notes -- Docker架设Nginx服务器

> SRC: <https://www.jianshu.com/p/2057576bfc5a>

看看已经有什么镜像
```
sudo docker search nginx
```

对目前（2017年2月）的Nginx来说，有以下常用镜像可供选择：

> - `1.11.10`, `mainline`, `1`, `1.11`, `latest` (mainline/jessie/Dockerfile)
> - `1.11.10-alpine`, `mainline-alpine`, `1-alpine`, `1.11-alpine`, `alpine` (mainline/alpine/Dockerfile)
> - `1.10.3`, `stable`, `1.10` (stable/jessie/Dockerfile)
> - `1.10.3-alpine`, `stable-alpine`, `1.10-alpine` (stable/alpine/Dockerfile)

我个人倾向于选择stable-alpine，它远比普通基于Debian制作的镜像要小得多。

## 使用docker-compose
```yaml
# vim: set shiftwidth=2 tabstop=2 softtabstop=-1 expandtab:

version: '2'
services:
  nginx:
    image: nginx:stable-alpine
    restart: unless-stopped
    network_mode: host
    volumes:
      - /srv/nginx/nginx.conf:/etc/nginx/nginx.conf
      - /srv/nginx/conf.d:/etc/nginx/conf.d
      - /srv/nginx/html:/usr/share/nginx/html
      - /var/log/nginx:/var/log/nginx
    ports:
      - "80:80"
    environment:
      - NGINX_HOST=your.domain
      - NGINX_PORT=80

```
将上述内容，写入一个docker-compose.yml文件。在同级目录，执行sudo docker-compose up，即可等价于一个超复杂的docker run ...。

docker-compose最擅长的是描述一组容器的配置与关系，同时启动或关闭。但如果单个容器的配置也很复杂，我也倾向于使用它。

这里，所有内容都放到主机的/srv/nginx目录下，让挂载进容器使用；而/var那边，相当于把log也映射出来。这样，在主机上也可以方便地修改配置与网页，并且可以查看log。

如果考虑到更方便的部署与迁移，甚至可以把Nginx的配置与docker-compose.yml放在同一个目录，组成一个Git库。volumes参数，也可改成相对路径。然后，三条命令就可以在一台机器上部署一个网站。
```
git clone git@server:repo/name.git Website
cd Website
sudo docker-compose up
```
这样的部署方案，如此简洁，以致优雅。

## Nginx的stable镜像的Dockerfile。
```docker
FROM debian:jessie

MAINTAINER NGINX Docker Maintainers "docker-maint@nginx.com"

ENV NGINX_VERSION 1.10.3-1~jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
    && echo "deb http://nginx.org/packages/debian/ jessie nginx" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y \
                        ca-certificates \
                        nginx=${NGINX_VERSION} \
                        nginx-module-xslt \
                        nginx-module-geoip \
                        nginx-module-image-filter \
                        nginx-module-perl \
                        nginx-module-njs \
                        gettext-base \
    && rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
```
该文件地址：
<https://github.com/nginxinc/docker-nginx/tree/master/stable/jessie>


