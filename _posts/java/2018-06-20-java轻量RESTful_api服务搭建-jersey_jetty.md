---
layout: post
title: [转帖] java轻量RESTful api服务搭建(jersey+jetty)
category: Java
tags: Java  Jersey
keywords: Java RESTful  Jersey
published: true
---

# [转帖] java轻量RESTful api服务搭建(jersey+jetty)

> Ref: https://www.jianshu.com/p/7c4b11096553

## REST（Representational State Transfer

> REST的六个特性：
> 
> - Client-Server：服务器端与客户端分离。
> - Stateless（无状态）：每次客户端请求必需包含完整的信息，换句话说，每一次请求都是独立的。
> - Cacheable（可缓存）：服务器端必需指定哪些请求是可以缓存的。
> - Layered System（分层结构）：服务器端与客户端通讯必需标准化，服务器的变更并不会影响客户端。
> - Uniform Interface（统一接口）：客户端与服务器端的通讯方法必需是统一的。
> - Code on demand（按需执行代码？）：服务器端可以在上下文中执行代码或者脚本？

## Codes

### Maven config
```xml
<dependency>
    <groupId>com.sun.jersey</groupId>
    <artifactId>jersey-servlet</artifactId>
    <version>1.19.4</version>
</dependency>

<!-- 对象自动转json -->
<dependency>
    <groupId>com.sun.jersey</groupId>
    <artifactId>jersey-json</artifactId>
    <version>1.19.4</version>
</dependency>

<dependency>
    <groupId>com.sun.jersey</groupId>
    <artifactId>jersey-client</artifactId>
    <version>1.19.4</version>
</dependency>

<dependency>
    <groupId>org.eclipse.jetty.aggregate</groupId>
    <artifactId>jetty-all-server</artifactId>
    <version>8.2.0.v20160908</version>
</dependency>
```

### Java
```java

/**
 * Copyright (C) 2017 The RDT of Wireless R&D in MIG. All right reversed. <p/> Created by vellhe on
 * 2017/7/7
 */

package com.tencent.awake.data.processing.service.rest;

import com.sun.jersey.spi.container.servlet.ServletContainer;
import com.tencent.awake.data.processing.db.mybatis.dao.model.SubTaskInfoPo;
import com.tencent.awake.data.processing.db.mybatis.dao.model.TaskInfoPo;
import org.eclipse.jetty.server.Server;
import org.eclipse.jetty.servlet.ServletContextHandler;
import org.eclipse.jetty.servlet.ServletHolder;

import javax.ws.rs.Consumes;
import javax.ws.rs.GET;
import javax.ws.rs.POST;
import javax.ws.rs.Path;
import javax.ws.rs.PathParam;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;
import java.util.Date;

/**
 * @author vellhe@tencent.com
 * @date 2017/7/7
 * @description 提供REST接口
 */
@Path("/")
public class RestInterface {

  /**
   * 根据id查询任务信息.
   *
   * @param id 任务id
   * @return 任务信息
   */
  @Path("/getTaskInfo/{id}") // 大括号里的是参数名，在函数位置使用@PathParam注解映射
  @GET // 声明这个接口必须GET访问
  @Produces(MediaType.APPLICATION_JSON) // 声明这个接口将以json格式返回
  public TaskInfoPo getTaskInfo(@PathParam("id") int id) {
    TaskInfoPo taskInfoPo = new TaskInfoPo();
    taskInfoPo.setId(id);
    taskInfoPo.setAppId("test");
    return taskInfoPo;
  }

  /**
   * 根据taskInfo查询subTaskInfo.
   *
   * @param taskInfoPo taskInfo
   * @return subTaskInfo
   */
  @Path("/getSubTaskInfo/") // url上没有参数，参数通过body传入
  @POST
  @Consumes(MediaType.APPLICATION_JSON) // 声明传入参数是json格式
  @Produces(MediaType.APPLICATION_JSON)
  public SubTaskInfoPo getSubTaskInfo(TaskInfoPo taskInfoPo) {
    SubTaskInfoPo subTaskInfoPo = new SubTaskInfoPo();
    subTaskInfoPo.setId((int) System.currentTimeMillis());
    subTaskInfoPo.setTaskId(taskInfoPo.getId());
    subTaskInfoPo.setCreateTime(new Date());
    return subTaskInfoPo;
  }

  /**
   * 测试用的main函数.
   */
  public static void main(String[] args) throws Exception {
    Server server = new Server(8282); // 监听8282端口
    ServletHolder servlet = new ServletHolder(ServletContainer.class);
    // 设置初始化参数
    servlet.setInitParameter("com.sun.jersey.config.property.resourceConfigClass", "com.sun.jersey.api.core.PackagesResourceConfig");
    servlet.setInitParameter("com.sun.jersey.config.property.packages", "com.tencent.awake.data.processing");
    servlet.setInitParameter("com.sun.jersey.api.json.POJOMappingFeature", "true"); // 自动将对象映射成json返回
    ServletContextHandler handler = new ServletContextHandler(ServletContextHandler.SESSIONS);
    handler.setContextPath("/");
    handler.addServlet(servlet, "/*");
    server.setHandler(handler);
    server.start();
    System.out.println("start...in 8282");
  }
}
```
运行后会监听8282端口（代码里），这段demo代码提供了两个接口，所以来看看怎么请求这两个接口吧. 我使用PostMan来模拟请求