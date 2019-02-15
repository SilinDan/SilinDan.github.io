---
title: 手把手教你使用hexo在github上建立个人网站,绕过所有的坑
tags:
- 标签1
- 标签2 (可选)
categories: 技术文章 # 分类
thumbnail: https://images.unsplash.com/photo-1524995997946-a1c2e315a42f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=500&q=60 # 略缩图
---

# 目录
- 环境配置
- hexo会有哪些坑
- 开始建立一个hexo博客
- 挑选一个你喜欢的主题
- 与github关联
- 创建分支
- 部署

---

# 环境配置

## 安装Node.js
下载 [Node.js](https://nodejs.org/download/)
## 安装Git
下载[git](http://git-scm.com/download/)




# hexo会有哪些坑

## hexo神坑的本质
在开始之前，意识到需要绕过哪些坑，是非常重要的，它可以帮我们节约不少时间。

在本地生成后, 将生成页面上传到网站上。应当有两个分支, 一个设置文件, 一个页面文件。
但是大家在第一次搭建博客时,会弄不清应当将页面文件放在哪个分支。

于是便出现各种各样奇怪的问题：
比如github发邮件给你告知失败：
> The page build failed for the master branch with the following error:
The tag fancybox on line 77 in themes/landscape/README.md is not a recognized Liquid tag. For more information, see https://help.github.com/articles/page-build-failed-unknown-tag-error/.

而我们要做的就是在一开始绕过这些坑，避免浪费不必要的时间。

---

# 开始建立一个hexo博客


## 安装Hexo
安装Hexo: [文档|Hexo](https://hexo.io/zh-cn/docs/index.html)
``` bash
$ npm install -g hexo-cli
```

## 建站
（此时会创建一个文件夹）

建站：[建站|Hexo](https://hexo.io/zh-cn/docs/setup.html)
```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

# 挑选一个你喜欢的主题
[hexo|主题](https://hexo.io/themes/)

打开上面的链接，挑选完毕后，进入该主题的github，将主题git clone下来。
- $ git clone [主题github地址] 
- config.yml 里 theme: 后面的 landspace 删掉.
- 主题文件夹里的 .git 要在这时就删掉.(.git是隐藏文件)
- 自带的主题 landspace 也删掉.



# 与github关联

## 新建远程仓库
 - 仓库名为：XXX (yourusername).github。 我的是:SilinDan.github.io
 - 在本地文件夹里设置git
 ```bash
 >> git init
 ```
 
 ## 本地与github仓库关联
 ```bash
 >>  git remote add origin http:// … (你的远程仓库链接)
 
 >> git push -u origin master
 ```
 
 # 创建gh-pages分支
 - 注意, 以后源代码都在这个分支
 
 创建并切换到此分支：
 ```bash
$ git checkout -b gh-pages
$ git push -u origin gh-pages
```

# 部署
hexo 部署到 github: [部署 | Hexo](https://hexo.io/zh-cn/docs/deployment.html)
- 在 config.yml 里添加 theme: 你的主题名
-  npm install hexo-deployer-git –save
-  修改_config 关于deploy部分内容

```yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
type: git
repo:  你的远程仓库 url`
(e.g.我的是: https://github.com/SilinDan/SilinDan.github.io  )`
branch: master
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://http://hexo.junyu.pro
root: /
permalink: :year/:month/:day/:title/
permalinkdefaults:
```

# 注意
- 配置文件永远在 gh-pages 分支, 不需转换分支到 master
- 转换分支前, 先 git add . /git commit 以防分支互相污染, 这将直接导致你回不去.
- 另外, 尝试添加新功能前, 另建分支尝试, 测试可用后再合并到 gh-pages 分支.

# 参考

[手把手教你使用Hexo + Github Pages搭建个人独立博客](https://segmentfault.com/a/1190000004947261)

[hexo 博客的神坑及本质原因](https://liguanghe.github.io/2017/11/06/blogRebuilt/)

[文档|heox](https://hexo.io/zh-cn/docs/index.html)