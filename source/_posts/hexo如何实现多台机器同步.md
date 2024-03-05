---
title: hexo如何实现多台机器同步
date: 2024-03-05 12:25:54
mathjax: true
tags:
    - hexo
categories:
    - hexo
---

## 思路

当我们用hexo d部署博客到git后，实际上是将md文件解析后的html发送到git仓库中，目录如下：

hexo如何实现多台机器同步![p1](2024/03/05/hexo如何实现多台机器同步/p1.png)

因此无法通过该仓库同步hexo环境

可以另起一个分支，将hexo环境提交到git分支上，或者另外建一个仓库， whatever。

此处新建一个分支xxx，在github上将默认分支设置为xxx，将hexo环境同步到分支上：

通过如下命令：

```code
git add .
git commit "back up(随便写)"
git push
```

同步成功后的git仓库：

![p2](2024/03/05/hexo如何实现多台机器同步/p2.png)

另一台同步环境的机器：

首先要安装git，nodejs

nodejs安装好cnpm

```code
npm install -g cnpm –registry=http://registry.npmmirror.com
```

从git远程仓库拉取hexo博客环境

```code
git clone xxx(你的仓库xxx分支地址)
cnpm install
hexo g
hexo s
```



注：

如果使用github，https协议可能无法连接，可以配置ssh

windows环境下

运行git bash

```code
cd ~/.ssh
ssh-keygen -t rsa -C "你的github账号邮箱" #生成ssh私钥公钥，一路确定
```

将/.ssh目录下的id_rsa.pub为公钥，添加到github上

使用ssh的链接拉取、同步即可