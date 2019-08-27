---
title: "如何用 hugo 搭建个人博客"
date: 2019-08-26T13:34:36+08:00
draft: false
---

# 如何用 hugo 搭建个人博客

## 为什么我们选择 Hugo

Hugo 是 go 语言实现的一个博客生成器，是世界上最快的博客生成器。JS 实现的博客生成器叫做 Hexo,但是非常难用，在学习过程中呢，工具不是重点，因为我们终将去尝试多种工具：）

## 一 下载并安装 Hugo

1. 前置条件 FQ 并且安装 chrome 插件 [Proxy SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif)
2. github 下载 [Hugo 安装包](https://github.com/gohugoio/hugo/releases)
3. 把 Hugo 解压并添加到 Path, 启动 cmder,运行 hugo version 查看版本。

## 二 构建 Hugo 项目

1. 进入 Hugo 官网，点击 quick start， follow her heart
2. 命令行输入`hugo new site xxx.github.io-creator`新建 Hugo 项目
3. 命令行输入`git init`初始化 git
4. 命令行输入`git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke`添加 ananke 博客主题
5. 一般在 post 文件夹下添加 markdown 文件，也就是我们的博客
6. 命令行输入`hugo server -D`启动 Hugo 服务器，浏览器进入`http://localhost:1313/`查看效果

## 三 上传 github

1.命令行输入`hugo`生成 public 文件夹，里面已经生成好了我们将要上传的文件

2.命令行进入 public 文件夹 输入

`git init`
`git add .`
`git commit -v`

3.在 github 上新建 github page，命名 xxx.github.io 4.命令行输入

`git remote add origin git@github.com:xxx/xxx.github.io.git`

`git push -u origin master`

4.将项目 master 分支推到 origin 的 master 分支

## 四 在 github 上绑定域名

1. 在 namesilo 上购买域名，
2. 在 github 上填入域名，点击 learn more ![图片](/images/1.png)
3. 下拉，点击 setting up an apex domain ![图片](/images/2.png)
4. 下拉找到 Configuring A records with your DNS provider ![图片](/images/3.png)
5. 将四个 ip 配置到 namesilo
6. 等待完成：）
