---
description: MarkDown=>HTML
---

# GitBook

GitBook是一个基于node.js的命令行工具，可以将MarkDown文章转为HTML、PDF等格式电子书，可以理解为一种文档转换工具（MarkDown=&gt;HTML）。

## 安装

预备软件：

* GitBook：转换工具
* Typora：编写MarkDown
* Git：版本控制

正式安装GitBook：

1. 安装node.js：GitBook基于node.js
2. 安装 gitbook

> npm install -g gitbook-cli

## 使用

### 初始化

在想创建书的文件下，执行如下命令

> gitbook init

默认会创建 README.md 和 SUMMARY.md

* README.md：用过git都知道，可以理解为简介
* SUMMARY.md：书的目录结构，gitbook会自动监听该目录，并创建缺失的文件\(利用`gitbook init`更新\)

### 转为网页

将Markdown格式的数据转为html，并借助node.js在本地搭建一个web页面

> gitbook serve

## Gitbook网页版

在gitbook的网页版上直接导入git工程，amazing好用~，本笔记后期转移到网页上编辑

## gh-pages

利用github的接口，将book传到gh-pages分支下，通过xxx.github.io/project\_name可直接访问书籍，区别于gitbook（github与gitbook的区别）

[将Gitbook上的书籍发布在GitHubPages上](https://blog.csdn.net/meiko_zhang/article/details/81350924)

