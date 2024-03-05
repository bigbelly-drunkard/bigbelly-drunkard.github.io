---
title: 使用hexo搭建个人博客
date: 2024-03-05 12:25:54
mathjax: true
tags:
    - hexo
    - 个人博客
categories:
    - hexo

---

先安装nodejs

node -v	#查看node版本
npm -v	#查看npm版本
npm install -g cnpm --registry=http://registry.npmmirror.com	#安装淘宝的cnpm 管理器
cnpm -v	#查看cnpm版本
cnpm install -g hexo-cli    #安装hexo框架
hexo -v	#查看hexo版本
mkdir blog	#创建blog目录
cd blog	 #进入blog目录
sudo hexo init 	#生成博客 初始化博客
hexo s	#启动本地博客服务
http://localhost:4000/	#本地访问地址
hexo n "我的第一篇文章" #创建新的文章 
\#返回blog目录
hexo clean #清理
hexo g #生成
\#Github创建一个新的仓库 YourGithubName.github.io
cnpm install --save hexo-deployer-git #在blog目录下安装git部署插件
\----
\#配置_config.yml 

```code
	# Deployment
	## Docs: https://hexo.io/docs/deployment.html
	deploy:
  		type: git
 		  repo: https://github.com/bigbelly-drunkard/bigbelly-drunkard.github.io.git
  		branch: master
```

hexo d	#部署到Github仓库里

走到这一步报错：

```code
Error: Spawn failed
  at ChildProcess.<anonymous> (/Users/lin/blog/node_modules/.store/hexo-util@2.7.0/node_modules/hexo-util/lib/spawn.js:51:21)
  at ChildProcess.emit (node:events:518:28)
  at ChildProcess._handle.onexit (node:internal/child_process:294:12)
```

将repo换成gitee，发现没有报错，初步鉴定为网络问题，忧郁gitee白嫖域名需要备案，太麻烦了，还是用github把。

试了好久终于找到解决方法，将https链接改为ssh连接，参考：https://github.com/hexojs/hexo/issues/2778

![picture1](2024/03/05/使用hexo搭建个人博客/picture1.png)

配置_config.yml ，将repo改为ssh链接

```code
# Deployment
## Docs: https://hexo.io/docs/deployment.html
	deploy:
  	type: git
 		repo: git@github.com:bigbelly-drunkard/bigbelly-drunkard.github.io.git
  	branch: master
```

本地生成ssh公钥

ssh-keygen -t rsa -b 4096 -C "你的github邮箱"

一路点确定

生成的公钥默认存在/var/root/.ssh/id_rsa下，将id_rsa.pub的内容拷贝的github上，点击Deploy keys >> Add deploy key：![picture2](2024/03/05/使用hexo搭建个人博客/picture2.png)

Hero d 部署博客，成功

https://YourGithubName.github.io/  #访问这个地址可以查看博客

\#修改hexo根目录下的 _config.yml 文件 ： theme: yilia

hexo c	#清理一下
hexo g	#生成
hexo d	#部署到远程Github仓库
https://YourGithubName.github.io/  #查看博客



hexo无法显示图片解决方法：

https://blog.csdn.net/weixin_44999716/article/details/112401495

