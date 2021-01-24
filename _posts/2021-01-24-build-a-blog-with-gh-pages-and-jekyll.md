---
layout: post
title:  "Github Pages and Jekyll 博客搭建指南"
date:   2021-01-24 20:04:03 +0100
categories: jekyll
---

Notes: \\
1) 默认已有一个GitHub账户; \\
2) 使用MAC运行环境

## 1 使用Github Pages及Jekyll建立基础功能博客站点

### 操作步骤

1.1 在GitHub中创建一个新的仓库，名字任意（无需特殊后缀），选择“public”，其余不勾选，如下图（在后续选用模板时，会带有模板使用的license）。

1.2 在本地安装Jekyll，参考如下教程。

[MAC Installation Tutorial](https://www.youtube.com/watch?v=WhrU9m82Wm8&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&index=2)

[Windows Installation Tutorial](https://www.youtube.com/watch?v=LfP7Y9Ja6Qc&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&index=3)

1.3 成功安装后，在Terminal中使用cd命令进入想要创建博客文件的目录，依次执行如下命令。主要参考：[Creating a Site Tutorial](https://www.youtube.com/watch?v=pxua_1vyFck&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&index=4)

{% highlight ruby %}
jekyll new test_blog # 创建一个默认配置的新博客站点文件
cd. your_blog_folder # 进入新创建的站点文件夹
bundle exec jekyll serve # 开启jekyll服务
{% endhighlight %}

成功运行后，打开浏览器，输入“localhost:4000”，能够显示新建博客的home界面。

1.4 配置\_config.yml文件，将其中的baseurl后填入1.1中建立的仓库名，如本站点的“/blog”。 


1.5 将本地的博客站部署至GitHub Page上，依次执行如下命令。主要参考：[Hosting on Github Pages Tutorial](https://www.youtube.com/watch?v=fqFjuX4VZmU&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&index=19)

{% highlight ruby %}
git init # 初始化本地仓库
git checkout -b gh-pages # 创建并切换至一个新的gh-pages分支
git status # 检查当前的仓库状态
git add . # 添加所有的文件
git commit -m "update cfg" # 提交，并添加描述
git remote add origin https://github.com/yujinghua/your_remote_repo_name.git 
# 将本地的仓库与1.1步骤中的远程仓库连接
git push origin gh-pages # 将本地文件上传
{% endhighlight %}

此时查看网页中的GitHub仓库，即可见新上传的文件。

1.6 查看GitHub仓库中，setting -> GitHub pages 项，即可看到站点发布相关的提示。当提示区域从蓝色变为绿色（显示“Your site is published at https://xxxx (<-发布的站点网址)”时，可通过该网址访问到页面）

1.7 完成上述部署后，即可实现，在本地编写博文，本地预览（localhost:4000，注意\_config.yml文件中的baseurl配置，需添加至地址中，如本站点预览地址为http://localhost:4000/blog/))。

完成本地编写后，使用git命令将新版本提交远程库，等待一段时间后，新文即可在目标网站发布（可通过setting -> GitHub pages中的发布提示查看是否完成发布）。

若直接在远程库中编写过文件后，记得及时使用git命令（如下）现将本地库中文件更新到最新，以免发生提交冲突（需要额外花时间解决冲突）。

{% highlight ruby %}
git pull origin gh-pages
{% endhighlight %}

### Ref
 
以上主要依据Mike Dane的[Jekyll - Static Site Generator Tutorial](https://youtube.com/playlist?list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB)系列教学视频 <- 推荐观看

## 2 Jekyll中Markdown语法 [todo]

## 3 Jekyll配置笔记 [todo]


