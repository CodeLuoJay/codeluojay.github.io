---
title: SpringBoot 快速入门案例
date: 2019-12-28 21:15:21
tags: SpringBoot
categories:  SpringBoot2.X系列
images: "https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Start/SpringBoot-start-index.jpg"
---

SpringBoot是一个配置很少就能轻松搭建Web应用框架，相信学过SSH或者SSM框架的开发者都知道在该框架环境下需要配置一堆XML配置文件才能实现搭建Web应用，学习完SpringBoot后，搭建Web应用会让你有丝滑般的畅快。

## SpringBoot2.2.2版本快速入门环境要求

目前[Spring官网](https://spring.io/projects/spring-boot/)官网正式发行的版本是2.2.2版本，在其[官方文档](https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/html/getting-started.html#getting-started)列出以下环境要求，本文也是基于2.2.2版本快速搭建入门的案例，所谓工欲善其事必先利其器，生产环境得搞起来。

| 工具   | 版本                      |
| ------ | ------------------------- |
| Maven  | 3.3+                      |
| Java   | 8    (即JDK1.8及以后版本) |
| Tomcat | 9.0                       |

## 通过IDEA的Spring initializer快速搭建SpringBoot

SpringBoot快速搭建的工具有STS(Eclipse编程环境下常用)，Spring initializer（IDEA常用）,而官方文档推荐使用Maven构建工具基于Pom.xml文件引入依赖构建，以上的工具和搭建方式这里就见仁见智。

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Start/SpringBoot-start-1.png)

点击Next-->输入包名和项目名-->Next

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Start/SpringBoot-start-2.png)

勾选Web模块-->Next

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Start/SpringBoot-start-3.png)

## Spring initializer创建的SpringBoot文件目录结构

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Start/SpringBoot-start-4.png)

这里只需要关注pom.xml和src文件夹下目录结构，只需要知道带有mvn的都是与Maven相关的，用于记录Maven版本信息和方便分布式部署，这里入门案例是单体部署，不需要用到，有强迫症可以删除带有mvn的文件和目录

| 对应的作用                                        | 文件夹                                            |
| ------------------------------------------------- | ------------------------------------------------- |
| 存放maven-wrapper.properties和其jar包（可以忽略） | .mvn                                              |
| 标准的Maven目录结构(其目录下有main和test文件夹)   | src                                               |
| main文件夹存放Java源文件，test存放用于测试文件    | src下的main                                       |
| SpringBoot自动配置默认扫描的目录文件夹            | src下的main的java下的com.xxx                      |
| 是SpringBoot程序主入口                            | src下的main的java下的com.xxx的xxxApplication.java |
| 用于存放静态资源文件如js,css.images               | src下的main的resources下的static                  |
| 用于存放模板引擎的文件如themeleaf，freemarker     | src的main的resources下的templates                 |

这里补充说明，Controller是需要手动创建在**xxxApplication**所在的文件夹下，每个人起的包名和项目名不同所以会有所区别，**因为该xxxApplication是程序主入口**，其所在文件夹是SpringBoot自动配置默认扫描的目录文件夹相当于之前的SSM框架中component-scan的作用，自动配置Controller。

## 编写控制层代码XXXController

```java
package com.luojay.springbootstart.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller//标注是控制层的注解
public class SpringBootController {
    @ResponseBody//以JSON格式输出到页面的注解
    @RequestMapping("/hello")//映射路径
    public String SayHello(){
        return "Hello SpringBoot!";
    }
}

```

## 启动XXXApplication访问映射路径

点击绿色三角形启动web程序

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Start/SpringBoot-start-5.png)

在浏览器输入http:localhost:8080/hello 显示字符串即为完成入门案例

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Start/SpringBoot-start-6.png)



