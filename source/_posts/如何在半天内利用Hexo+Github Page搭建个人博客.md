---
title: 如何在半天内利用Hexo+Github Page搭建个人博客
images: "https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/hexo/hexo_backgroud.jpg?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1567575858;1630647858&q-key-time=1567575858;1630647858&q-header-list=&q-url-param-list=&q-signature=a563a5f4b5a22568079359e412762481aada7129"
---
作为一个程序员，个人觉得博客和Github用来记录自己的学习历程最为合适不过，相信大部分的学习编程的人应该都会用过Github,而Github Page 依托着Github的项目适合用来展示项目的Demo。这里我们用来Hexo+Github Page搭建个人博客最适合不过了。

###	需要具备的知识
		1.Git的基本命令知识
		2.Windows的基本命令（DOS命令）
		3.爱折腾的意识（其实就很简单但需要踩坑爬坑）
		4.一定的搜索学习能力

###	需要用到的工具
		1.Node.js(6.9版本以上)
		2.Git&Github
		3.Hexo

###	（一）安装Node.js
在Windows操作系统下，按下win+R 后输入 cmd 调出 windows命令行终端查看Node.js版本
```bash
node -v 
```
如果显示版本不是6.9或以上，请重新安装。如果显示不是内部处理文件，则未安装Node.js
[Node官网](https://nodejs.org/en/)	下载安装node.js

安装完Node.js后需要配置一下windows环境变量，在我的电脑-->高级系统设置-->高级-->环境变量 

 配置Path环境变量以便DOS命令能够运行Node.js命令

Path环境变量中加入你安装Node目录的node.exe所在的目录，这里我安装在G盘下

```bash	
;G:\Instrallations\nodejs
```

再次检查Node.js版本，在Dos中输入下面的命令

```bash	
node -v
```

显示的版本号在6.9版本或以上即为完成Node.js安装

###	（二）安装Git

[Git官网](https://git-scm.com/)		下载安装Git
可以自行下载安装，如果安装步骤不会可以百度或者可以参考博文	[Git安装和使用](https://www.cnblogs.com/ximiaomiao/p/7140456.html)

安装好Git之后，需要在	我的电脑-->高级系统设置-->高级-->环境变量  配置Path环境变量
这里以我安装的Git目录为例，请自行替换路径
```bash
;G:\Instrallations\Git
```
然后在windows dos命令行中输入以下命令 是否显示git的版本信息 是则表示安装成功
```bash 
git --version
```

###	（三）创建Github和Github Page
去[Github官网](https://github.com/) 注册一个Github账号，需要注意邮箱必须填常用的，它是Github用来发提示信息和推送信息给你的，在忘记密码后也是通过邮箱找回

接着去[Github Page官网](https://pages.github.com/)	按照官网的详细教程 创建个人项目为主页的静态页面。（此处英文水平不是很好的建议使用网页的翻译为中文）

###	（四）Git与Github设置SSH,通过Git把Github刚刚创建的项目克隆到本地库

在Git Bash中输入以下命令，如果显示：No such file or directory 则表示本地没有SSH秘钥，需要我们新建一个SSH

```bash
$ cd ~/.ssh
```

生成本地的SSH,这里邮箱地址可以输入自己的邮箱地址，这里的「-C」的是大写的「C」

```	bash 
$ ssh-keygen -t rsa -C "邮件地址@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key(/Users/your_user_directory/.ssh/id_rsa):<回车就好>	
```

然后设置一个密码（后面使用Git提交到Github的时候会用到，这里设置好请记录下来）

```bash 
Enter passphrase (empty for no passphrase):<输入加密串>
Enter same passphrase again:<再次输入加密串>
```

注意：因为Linux 命令(Git Bash)下输入密码是无提示(即不会显示*******)，这里直接输入即可，输入完按回车。

检查是否生成SSH秘钥的方法：打开C:\Users\Administrator\目录看是否存在id_rsa以及id _rsa.pub文件，存在则生成成功

###	（五）Git通过SSH连接到Gthub
登录Github官网点击右上角的 Account Settings--->SSH Public keys ---> add another public keys
打开上面目录的 id_rsa.pub 文件,全选里面的秘钥内容，复制到Github的Key文本框 点击 add key 
key上面的标题随便起一个名字即可。

测试是否连接成功，输入下面的命令
```bash
$ ssh -T git@Github.com
```
接着会提示你输入之前设定的id_rsa的密码，设定密码是为了保障Github与Git的传输安全
```bash
Enter passphrase for key '/c/Users/Administrator/.ssh/id_rsa':
```
如果查看结果输出以下信息，则表示成功连接
```bash
Hi bobi8344! You've successfully authenticated, but GitHub does not provide shell access
```
###	（六）安装Hexo
[hexo官网](https://hexo.io/)  安装hexo完全是参考官方的文档，包括后面的设置主题等都是参考官方的文档，这里建议最好是看英文文档，因为网页翻译有时候会翻译得不准确。

在Windows中执行以下命令安装Hexo
```bash
npm install hexo-cli -g
```

安装完后，会在C:\Users\Administrator\AppData\Roaming\npm (以我的win10为例）目录下生成
node_modules文件夹以及hexo.cmd和hexo，非win10在Dos查看具体目录验证是否成功安装hexo

安装完Hexo后需要对Hexo进行初始化，官方的文档在blog文件中初始化，这里建议新建在自己方便管理的文件夹例如我在H盘新建hexo文件夹，然后通过Git Bash转到这个文件夹再执行初始化命令

```bash
$ hexo init blog
$ cd blog
$ npm install
```
初始化完成后会在blog目录下生成以下文件夹及文件

├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes

然后执行以下命令，在浏览器输入<http://localhost:4000/>就可以查看本地生成博客模板

```bash
$ hexo g 
$ hexo s 
```
做到这一步就基本上已经完成百分之八十
###	(七)克隆Github远程仓库到本地库并安装Hexo部署插件
在blog下运行Git Bash，执行下面的命令,把clone后面的地址替换成要克隆的仓库的地址
```bash 
git clone https://github.com/bobi8344/bobi8344.github.io.git
```
接着安装Hexo部署插件，一个hexo和Git关联的插件，win+r 输入cmd,调出终端窗口执行下面命令
```bash 
npm install hexo-deployer-git --save
```
修改根目录下的 _config.yml 配置文件，注意这里必须使用SSH的方式（前面配置过）提交，否则会提交不了，因为在hexo2.0版本后，https的方式好像有Bug,我也踩了这个坑好久才跳出来
```bash 
deploy:
  type: git
  repo: git@github.com:bobi8344/bobi8344.github.io.git
```
最后在Git Bash执行Hexo部署命令，部署到Github上，打开<https://bobi8344.github.io/>查看刚才部署的博客
```bash 
hexo d
```

