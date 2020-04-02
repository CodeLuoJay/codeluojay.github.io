---
title: Spring Boot入门案例修改默认配置
date: 2019-12-28 21:23:21
tags: SpringBoot
categories:  SpringBoot2.X系列
images: "https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Study/SpringBoot-index.jpg"
---

​		Spring Boot官方声称搭建Web应用开箱即用，其根本原因就是底层封装好大部分的约束和配置，而作为合格的开发者，肯定要对这些配置有点好奇心，修改定制成自己的Web应用才能用得舒服。最近在学习SpringBoot时搜索网上的一些教程看到修改Banner的教程，也尝试了一把，记录这个好玩的东西。
>## 修改Spring Banner
>

​		首先来玩一个好玩的东西，就是修改Spring Boot默认的Banner，默认启动应用会输出Spring Banner如果我们自己做些小Demo，加上个性化Banner，可能会我们的程序更加逼格一点呢，早在Spring Boot 1.x 版本中就已经有了更换启动Banner的方法，并且使用起来非常简单。

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Banner/SpringBoot-Banner-banner.png)

>### :triangular_flag_on_post:个性化修改Banner案例一：ASCII文字版本Bannner	

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Banner/SpringBoot-Banner-SteveJobs.png)

>### :triangular_flag_on_post:个性化修改Banner案例二：Image转ASCII文字版本Bannner

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Banner/SpringBoot-Banner-luojay.png)

>### :triangular_flag_on_post:个性化修改Banner案例三：脑洞新奇的佛祖Bannner

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Banner/SpringBoot-Banner-fozu.png)


>#### :bookmark_tabs: 个性化修改Banner修改步骤

1. 在src/main/resources路径下新建一个banner.txt文件

2. 在下面三个自定义的Banner的网站转换好Banner相关的ASCII字符

3. 在banner.txt中填入转换好Banner相关的ASCII字符即可

   

![SpringBoot-Banner-Path](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Banner/SpringBoot-Banner-Path.png)



>#### :link: 自定义Banner网站链接

[文本转ASCII神奇网站](http://patorjk.com/software/taag)

[ascii生成器](http://www.network-science.de/ascii/)

[图片转TXT](http://www.degraeve.com/img2txt.php)



>#### :link:有关Banner的修改比较好的博文推荐(引用)

[【程序猿DD 】新年彩蛋：Spring Boot自定义Banner](http://blog.didispace.com/spring-boot-banner/)

[Srpingboot启动彩蛋banner修改（让springboot多点乐趣）](https://blog.csdn.net/A924110137/article/details/91797859)



>### Spring使用YAML修改Tomcat端口

​		SpringBoot使用一个全局的配置文件，配置文件名是application(固定),但application.properties和application.yml两种格式，学过JavaSE的基本都会porperties文件配置，这里来讲讲**YAML**文件



> ### YAML与XML类比

​		**YAML**是一种和xml类似但比xml简洁的标记语言文件，**yml**是**YAML**文件的后缀名。

下面分别以xml和yml写一段同样功能的标记语言来表示Tomcat服务器端口

```xml
//xml文件记录服务端口为8080
<server>
	<port>8080</port>
</server>
```

```yml
//yaml文件记录服务端口为8080
server:
	port:8080
```

​		通过上面的代码比较，就可以看出，yml用两个标签和两个冒号就能完整表示一个服务端口，而xml文件则需要四个标签才能达到同样的功能，所以在简洁程度完爆xml文件，后来被SpringBoot推荐用作来记录配置信息的文件

> ### YAML与Properties类比

​		分别先上一段properties和yml表示数据库连接的配置信息，对比你会发现yaml比porperties更加简洁。

```properties
###properties config mysql info
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/user?characterEncoding=utf8
jdbc.username=luojay
jdbc.password=luojay
```

```yaml
###yaml config mysql info
jdbc:
	driver:com.mysql.jdbc.Driver
	url:mysql://localhost:3306/user?characterEncoding=utf8
	username:luojay
	password:luojay
```

>### YAML语法

​		之所以跟porperties类比，是因为yaml和porperties语法有类似，都是用键值对(key-value)来表示一个配置信息项

不同的点在于：

1.键值对匹配的符号不同

porperties用**等于号**匹配键和值 		例如`username=luojay`

yaml用**冒号**匹配键和值 					例如`username:luojay`

2.yaml严格匹配缩进来表示层级关系

```yaml
###username和password在jdbc层级下是同一层级关系
jdbc:
	username:luojay
	password:luojay
###username和password在jdbc层级下不是同一层级关系
jdbc:
	username:luojay
		password:luojay
```

3.yaml严格区分大小写，大小写不同的属性和值是不同的。

```yaml
USERNAME:LUOJAY 
username:luojay
###区分大小写luojay是不同的属性值
```



​		这里以application.yml为例修改端口值，SpringBoot默认的Tomcat是内嵌在Jar包中且端口是8080 修改配置文件的作用：SpringBoot在底层都给我们自动配置好，通过修改端口可以解决开发中常见的端口被占用的冲突问题，可以同时部署多个应用在多个Tomcat上

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Banner/SpringBoot-Banner-Port.png)


启动项目查看控台日志，查看端口是否已经改好

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/SpringBoot-Banner/SpringBoot-Banner-log.png)



> ###  文章配套的源码已传到Github:sparkles:

[SpringBoot-Banner](https://github.com/bobi8344/SpringBoot-Study/tree/master/springboot-banner)

欢迎clone 如果有帮助，请给个start！

