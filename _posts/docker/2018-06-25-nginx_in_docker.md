---
layout: post
title: Nginx in Docker
category: Docker
tags: Docker  Web
keywords: Docker  Nginx
published: true
---

# Nginx in Docker

> REF: <http://www.ruanyifeng.com/blog/2018/02/nginx-docker.html>

## 默认运行
```
docker container run  -d  -p 127.0.0.1:8080:80  --rm  --name ngweb  nginx
```
-d：在后台运行
-p ：容器的80端口映射到127.0.0.2:8080
--rm：容器停止运行后，自动删除容器文件
--name：容器的名字为mynginx

### Try it
<http://127.0.0.1:8080>


## 映射网页目录
```
docker container run  -d  -p 127.0.0.1:8080:80  --rm  --name ngweb  -v ~/wy/codes/docker/nginx/html:/usr/share/nginx/html   nginx
```

## 复制配置目录到本地机器
```
docker container cp ngweb:/etc/nginx .   # 把mynginx容器的/etc/nginx拷贝到当前目录。不要漏掉最后那个点。
```

## 映射配置目录
```
docker container run  -d  -p 127.0.0.1:8080:80  --rm  --name ngweb  -v ~/wy/codes/docker/nginx/html:/usr/share/nginx/html  -v ~/wy/codes/docker/nginx/conf:/etc/nginx   nginx
```

## 修改nginx.conf的配置

> REF: <http://kael-aiur.com/docker/%E5%9C%A8docker%E4%B8%8A%E8%BF%90%E8%A1%8Cnginx.html>

将
```
location / {
    root   /usr/share/nginx/html;
    index  index.html index.htm;
}
```
修改为：
```
location / {
    proxy_pass http://192.168.85.1:8099;
}
```
即所有根目录的请求全部转发到<http://192.168.85.1:8099>这个地址来。