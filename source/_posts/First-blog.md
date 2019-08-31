---
title: Get Start My Hexo Blogs Way
date: 2019-08-14 23:43:45
excerpt: 第一篇写在自己站点的博客，具有划时代意义。基于Hexo博客框架与简洁的Clover主题，托管在交友平台GitHub上。
photos: [https://i.loli.net/2019/08/18/wuaTin5ZbDSQjHE.gif]
tags:
  - 随笔
  - Hexo
  - 里程碑
---

> 这第一篇具有划时代意义的文章，单纯写一写我这即将踏上创作旅程的澎湃之心。

自开始学习前端的第一天起，无论是 Google 还是百度，所领略到的大多技术文章都来自`CSDN`、`掘金`、`思否`、`简书`。对，在此特别安利一下**掘金——我的知识朋友圈**。无可否认这些开发者社区在无数场景替我解决了难题，亦或是给予我灵感。也曾不止一次的期待像他们那样，在这些平台笔耕不辍，既内化了知识，又帮助了他人。

但我还是希望拥有我自己的个人站点，用我的手艺，我的一生，打造我的作品，我的一片天。

**目的不是为了流量和人气，而是内省和学习。**

我知道持续做好自己喜欢的事，该来的也许会迟到，但绝不会缺席。

如果读者有幸能来到我的站点，可以给你一些帮助，解决某个问题，不用客气，那是我的运气。

### 致敬

- [Hexo 官方教程](https://hexo.io/zh-cn/)
- [UI 设计师 Clover 的主题作品](https://github.com/esappear/hexo-theme-clover)
- [手把手教 GitHub + Hexo 搭建博客](https://chars.tech/blog/build-blog-by-hexo/)
- [用两个分支写博客](http://dxjia.cn/2016/01/27/hexo-write-everywhere/)

### 搭建概要

如果你也想要搭建一个这样的博客，完全可以立即行动以来，不要畏惧，一行代码都不用写。

#### #需要准备

安装`Node.js`（附带安装`npm`）

基本的`html`、`css`、`JavaScript`知识

`GitHub`账号

基本的几条`Git`命令或者相关工具

#### #开始搭建

1. 将`Node.js`环境安装到本地计算机，去官网下载一直下一步即可

2. 将`Git`工具安装到本地，Windows 安装 git bash

3. 进入[Hexo 官网](https://hexo.io/zh-cn/)，了解并读文档，读取按照步骤

4. 在命令行通过此命令安装 Hexo

   `npm install hexo-cli -g`

5. 初始化博客项目目录，搭建初始结构

   ```bash
   $ hexo init <folder>
   $ cd <folder>
   $ npm install
   ```

   ```
   -- 初始博客目录结构
   .
   ├── _config.yml
   ├── package.json
   ├── scaffolds
   ├── source
   |   ├── _drafts
   |   └── _posts
   └── themes
   ```

6. 在 `_config.yml`中配置你特有的参数，官网文档很详细

7. 本地预览

   ```
   $ hexo server
   // 浏览器打开localhost:4000即可预览
   ```

#### #主题替换

不用说，Hexo 默认的主题没人愿意用，除非你只关注于文章的内容，又或者真的是想着`别人有的我也要有`。

[Hexo 官网](https://hexo.io/themes/)有几百个开源的主题供你尽情挑选，看到喜欢的，就去里面的 GitHub 仓库下载下来，添加到博客目录下的`themes`文件夹下，再到配置文件里面将主题名字替换为新的主题名字即可。

除了官网，GitHub 上还有更多私人定制好的开源主题，你只需要 Google 一下。

每个主题都是一个小项目，是一个静态网站的模板，同样几乎都有功能介绍、使用文档，以及 demo 演示。只要文章不变，网站的样式可以根据主题一键更换，可以说是非常的友好啦。

#### #常用命令

```bash
常用命令：

hexo init 					#初始化一个目录
hexo new "postName" 		#新建文章
hexo new page "pageName" 	#新建页面
hexo generate 				#生成网页，可以在 public 目录查看整个网站的文件
hexo server 				#本地预览，'Ctrl+C'关闭
hexo deploy 				#部署.deploy目录
hexo clean 					#清除缓存，强烈建议每次执行命令前先清理缓存，每次部署前先删除 .deploy 文件夹

简写：
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

#### #日常管理

**强烈建议：**[用两个分支写博客](http://dxjia.cn/2016/01/27/hexo-write-everywhere/)

将部署上线的版本放在 GitHub 仓库下的`master`分支下，将 hexo 写作配置环境放在`hexo`分支下。

在项目的配置文件中配置部署参数：

```bash
deploy:
  type: git
  repo: https://github.com/dxjia/dxjia.github.io.git
  branch: master
```

日常本地 hexo 分支，这样每次部署时，自动更新到 master 分支下，而不会对 hexo 分支造成影响。也方便在不同的电脑中写作。

### 未来迭代

**Hexo + GitHub + Typora** 三者合一，用 MarkDown 语法随心所欲地写作，简直无懈可击，除了那死慢的 GitHub 令人恶心之外。还有无数可定制化的主题，可以满足很多天马行空的想象了。

至今还在实习期，闲下来的时间不多，技术功底也不是很强悍。在短期内这个作品处于 `DoubleYao 1.0`阶段。

**后期规划： DoubleYao 2.0**

> - 独立设计，全新的 UI 界面和交互方式
> - 随着文章数量的增多，增加 tags 标签和分类
> - 模块：技术 + 随笔 + 手册 + 工具

余生慢慢打磨。
