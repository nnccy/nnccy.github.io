---
title: git使用总结
categories:
  - - 工具
tags:
  - git
abbrlink: e90cddc
copyright: true
date: 2020-10-05 16:49:44
---

用来记录自己使用git中容易忘记的点，常年更新

<!--more-->

# git使用总结

## git基本操作

```c
/*把这个目录变成Git可以管理的仓库*/
git init 

/*把文件添加到仓库*/
git add readme.txt  //加入readme.txt
git add .  //加入目录下所有文件

/* 把文件提交到仓库*/
git commit -m "提交的说明"

/*将本地改动提交到远程*/
git push origin master

/*关联远程仓库*/
git remote add origin git@github.com:nnccy/learn_django_vue.git

/*从远程库克隆代码*/
git clone git@github.com:nnccy/learn_django_vue.git

/*从远程库拉取代码*/
git pull

```

## 密钥操作

- 查看本机是否已存在本地公钥`cat ~/.ssh/id_rsa.pub`

1. 否->生成   `ssh-keygen -t rsa -C "<您的邮箱>"`
2. 是-> 拷贝到剪贴板  `clip < ~/.ssh/id_rsa.pub`

- 在github远程设置加入本机ssh密钥,按照下图粘贴密钥

![截图](截图.png)


## 配置忽略文件

忽略操作系统自动生成的文件，比如缩略图等；
忽略编译生成的中间文件、可执行文件等
创建`.gitignore`文件，在文件写如下内容，


-  忽略 .idea文件夹,以及后缀为.ini的文件

```
.idea/
*.ini
```
