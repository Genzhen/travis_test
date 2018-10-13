---
title: github和hexo一步步搭建自己的博客
date: 2018-08-14 10:35:37
tags: [博客, github, hexo]
categories: note
---
** {{ title }}：** <Excerpt in index | 首页摘要>
博客搭建
<!-- more -->
<The rest of contents | 余下全文>
# 前言
**使用github pages服务搭建博客的好处：**
> 1.全是静态文件，访问速度快；   
> 2.免费方便，不需要服务器不需要后天；   
> 3.可以随意绑定自己的域名；   
> 4.数据绝对安全，基于github的版本管理，想恢复到哪个历史版本都行；   
> 5.博客内容可以轻松打包、转移、发布发布到其它平台。
> 6.……
## 准备工作   
在开始一切之前，你必须已经：   
* 有一个github账号   
* 安装了node、npm,并了解相关知识
* 安装了git
# 搭建github博客
## 创建仓库   
新建一个名为***你的用户名.github.io***的仓库，比如你的github用户名是***user***,那么新建的仓库名就是***user.github.io***（必须是你的用户名，否则无效），将来你的网站就可以通过http://user.github.iof访问了。   
## 绑定域名   
不绑定域名也是也是可以的，可以用默认的***username.github.io***来访问，如果想要拥有一个自己的域名也是可以的。   
首先需要你注册一个域名，阿里云什么的都可以，价格也不贵。    
域名配置最常见的有两种方式，CNAME和A记录，CNAME和A记录填写IP,IP可以通过ping一下***你的用户名.github.io***的IP。   
然后到你的github项目根目录下新建一个CNAME的文件，里面填写你的域名。
# 配置SSH Key
为什么要配置这个，因为你提交代码肯定要拥有你的github权限才可以，但是直接使用用户名和密码太不安全了。所以使用ssh key来解决本地和服务器连接问题。   
在终端中执行以下命令：   
`cd ~/.ssh`   
如果提示：no such file or directory说明你是第一次使用git。需要执行下面的命令：   
`ssh-keygen -t rsa -C "邮件地址""`   
然后连续回车三次，最终会生成一个文件在用户目录下，打开用户目录，找到.ssh/id_rsa.pub文件，记事本打开复制里面的内容，打开你的github主页，进入个人设置：   
***SSH and GPG keys->New SSH key***   
将刚复制的内容粘贴到key那里，title随便填，保存。   
## 测试是否成功   
`ssh -T git@github.com`   
如果提示Are you sure you want to continue connecting(yes/no),输入yes,然后会看到：   
> Hi username! You've successfully authnenticated,but Github does not provid shell access.   
看到这个信息，说明SSH已配置成功。   
# 使用hexo博客   
## 简介
hexo是一个简单、快速、强大的基于Github Pages的博客发布工具，支持Markdown格式，有众多优秀插件和主题。   
## 安装   
`npm install hexo -g`    
## 初始化  
在电脑的某个地方新建一个为名blog的文件夹（名字随意取）   
```
cd  blog的路径
hexo init
hexo g #生成
hexo s #启动服务

```
执行以上命令后，hexo就会在public文件夹生成相关html的文件，这些文件将来是要提交到github上去的。   
这是访问***http://localhost:4000***就可以看到内容了。
## 修改主题   
默认的主题很丑，我们可以自己选一个好看的，我选的是[balck-blue](https://github.com/WangGenzhen/black-blue)   
首先下载这个主题：   
```
cd blog路径
git https://github.com/WangGenzhen/black-blue.git themes/black-blue
```
修改_config.yml中的theme:landscape改为theme:black-blue,重新执行`hexo g`来重新生成。
## 上传到github   
如果一切都配置好了，一句`hexo d` 就可以。   
* 首先SSH key要配好   
* 其次,配置_config.yml中有关deploy的部分    
```angular2html
deploy:
    type: git
    repository: git@github.com/username/username.github.io.git
    branch: master
```
此时如果执行hexo d，一般会报错   
原因是还需要安装一个插件：   
```angular2html
npm install hexo-deployer-git --save
``` 
## 安装成功
现在访问http://username.github.io，就可以看到搭建好的博客了。
# 参考地址   
[Hexo和github打造个人博客](https://geeksblog.cc/hexo-githup-blog.html)   
[Hexo自用黑色主题](https://geeksblog.cc/hexo-theme.html)