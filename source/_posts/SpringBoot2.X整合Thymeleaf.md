Thymeleaf是适用于Web和独立环境的现代服务器端Java模板引擎，SpringBoot官方推荐使用的一个模板引擎

如果你对模板和模板引擎没什么概念的话，可以简单理解为Thymeleaf是一个高级简洁的JSP

如果学过MVC设计模式，那么Thymeleaf就是视图层（view）的主要核心内容
### 模板引擎是什么
模板引擎是为了使用户界面与业务数据（内容）分离而产生的，它可以将特定格式的模板和数据通过模板引擎渲染就会生成一个标准的HTML文档页面

它最早出现是C#语言用来渲染成.asp，所以它不是什么新鲜事了，Thymeleaf则是Java的一个模板引擎，与Thymeleaf类似的模板引擎还有Velocity和FreeMarker

### 模板引擎工作原理

![](https://imgkr.cn-bj.ufileos.com/2af67ab9-f9f2-4dfc-8eac-4cc98ee3566e.jpg)
按照我自己理解模板引擎只是将模板(Template)和数据(Java Object)渲染成网页(Html)的工具

JSP知道吧，它里面参杂Java内置对象代码来进行数据显示，而Thymeleaf按照我的理解就像去除了Java代码的简化版JSP
### 为什么要整合Thymeleaf

在SpringBoot2.X系列第一篇文章中整合的案例，各位应该有印象我没有去配置过Tomcat

因为SpringBoot将项目打包成**Jar包方式在内部集成的Tomcat运行**，传统JSP是要打成**war包**才能运行，所以这里不适用JSP必须整合Thymeleaf

使用Thymeleaf有助于前后端协同开发，因为它在无网络环境下也可以运行，前端程序员便于在静态页面查看页面效果，后端开发者也便于在服务器查看实时的数据交互

Thymeleaf严格遵循的mvc设计模式，不用像JSP页面大量嵌套Java代码，开发更简洁
### SpringBoot整合Thymeleaf
整合Thymeleaf需要Web模块和Thymeleaf模块相应的依赖Jar包
### pom.xml
```xml
<dependencies>
        <!--thymeleaf模块依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <!--Web模块依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
</dependencies>
```
![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Themeleaf/SpringBoot-Thymeleaf-Module.png)
在SpringBoot Initializer中分别勾选Web和Thymeleaf



### 整合Thymeleaf的项目结构
![SpringBoot整合Thymeleaf的目录结构](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Themeleaf/20191226221317.png)
在SpringbootApplication所在的包下所有Bean类会被SpringBoot自动扫描组装，static目录是SpringBootyong用来加载CSS,JS,IMG等静态资源，templates是用来放置html模板文件

### ThymeleafController
```Java
package com.luojay.springbootthymeleaf.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class ThymeleafController {
    @RequestMapping("/getThymeleaf")
    public String Welcome(){
        return "luojay";
        //此处返回值，对应templates的文件名，SpringBoot根据它找到对应Html
    }
}
```
@Controller注解是告知SpringBoot此类是控制层需要自动注入，@RequestMapping是映射访问路径注解，在localhost:8080之后的路径就是根据此处写的

### luojay.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>luojay's Thymeleaf</title>
</head>
<body>
    hello,luojay!
</body>
</html>
```

### 页面展示效果

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Themeleaf/SpringBoot-Thymeleaf-runResult.png)

### 进阶的学习案例

上面的入门例子可以说是简单到不行，有了这个基础，可以利用网上一些静态资源整合Thymeleaf搭建好看的页面,比如登录界面
![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Themeleaf/SpringBoot-Thymeleaf-login.gif)




### 文章配套代码
文章的配套源码已收录在我的GitHub仓库SpringBoot-Study中，点击阅读原文可以直达我的Github中的SpringBoot-Study仓库，如果案例的代码对你有帮助，欢迎star和fork！
Github:[springboot-thymeleaf](https://github.com/bobi8344/SpringBoot-Study/tree/master/springboot-thymeleaf "springboot-thymeleaf")

登录页面源代码：[**基于Layui简约登录界面**](https://www.17sucai.com/pins/34125.html "**基于Layui简约登录界面**")