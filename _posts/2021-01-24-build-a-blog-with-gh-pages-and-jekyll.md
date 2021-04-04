---
layout: post
title:  "博客搭建指南(Github Pages and Jekyll)"
date:   2021-01-24 20:04:03 +0100
categories: tool jekyll
---


## 内容提要
1. 使用Github Pages及Jekyll建立基础功能博客站点
1. 更改Jekyll主题
1. Jekyll中的Markdown语法

---


## 使用Github Pages及Jekyll建立基础功能博客站点

### 操作步骤如下

1.1 在GitHub中创建一个新的仓库，名字任意（无需特殊后缀），选择“public”，其余不勾选，如下图（在后续选用模板时，会带有模板使用的license）。

1.2 在本地安装Jekyll，参考如下教程。

[MAC Installation Tutorial](https://www.youtube.com/watch?v=WhrU9m82Wm8&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&index=2)

[Windows Installation Tutorial](https://www.youtube.com/watch?v=LfP7Y9Ja6Qc&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&index=3)

1.3 成功安装后，在Terminal中使用cd命令进入想要创建博客文件的目录，依次执行如下命令。主要参考：[Creating a Site Tutorial](https://www.youtube.com/watch?v=pxua_1vyFck&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&index=4)

``` ruby
jekyll new test_blog # 创建一个默认配置的新博客站点文件
cd. your_blog_folder # 进入新创建的站点文件夹
bundle exec jekyll serve # 开启jekyll服务
bundle exec jekyll serve --draft # 开启jekyll服务，在本地预览中显示draft posts
```

成功运行后，打开浏览器，输入“localhost:4000”，能够显示新建博客的home界面。

1.4 配置\_config.yml文件，将其中的baseurl后填入1.1中建立的仓库名，如本站点的“/blog”。 


1.5 将本地的博客站部署至GitHub Page上，依次执行如下命令。主要参考：[Hosting on Github Pages Tutorial](https://www.youtube.com/watch?v=fqFjuX4VZmU&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&index=19)

``` ruby
git init # 初始化本地仓库
git checkout -b gh-pages # 创建并切换至一个新的gh-pages分支
git status # 检查当前的仓库状态
git add . # 添加所有的文件
git commit -m "update cfg" # 提交，并添加描述
git remote add origin https://github.com/yujinghua/your_remote_repo_name.git 
# 将本地的仓库与1.1步骤中的远程仓库连接
git push origin gh-pages # 将本地文件上传
```

此时查看网页中的GitHub仓库，即可见新上传的文件。

1.6 查看GitHub仓库中，setting -> GitHub pages 项，即可看到站点发布相关的提示。当提示区域从蓝色变为绿色（显示“Your site is published at https://xxxx (<-发布的站点网址)”时，可通过该网址访问到页面）

1.7 完成上述部署后，即可实现，在本地编写博文，本地预览（localhost:4000，注意\_config.yml文件中的baseurl配置，需添加至地址中，如本站点预览地址为http://localhost:4000/blog/))。

完成本地编写后，使用git命令将新版本提交远程库，等待一段时间后，新文即可在目标网站发布（可通过setting -> GitHub pages中的发布提示查看是否完成发布）。

若直接在远程库中编写过文件后，记得及时使用git命令（如下）现将本地库中文件更新到最新，以免发生提交冲突（需要额外花时间解决冲突）。

``` ruby
git pull origin gh-pages
```

### Reference
 
以上主要依据Mike Dane的[Jekyll - Static Site Generator Tutorial](https://youtube.com/playlist?list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB)系列教学视频 <- 推荐观看

## 更改Jekyll主题

本站点当前使用的主题为[no-style-please](https://github.com/riggraz/no-style-please#customize-the-menu)，可根据其readme中的[installation](https://github.com/riggraz/no-style-please#installation)指导进行安装。

若为新建博客站点（个人内容为空），即可直接download或fork该项目后在其基础上更改。

该主题相关的配置操作，详见项目readme中的[Usage](https://github.com/yujinghua/blog#usage)。


## Jekyll中的Markdown语法

使用Jekyll撰写博文时，主要使用Markdown。根据所使用的Markdown解析器，可能语法略有不同。以下为本站点下使用的解析器(kramdown)语法。

### 1) 文本格式

**加粗**  \\
_倾斜_   \\
~~删除线~~   \\
转义字符，在特殊字符前添加下划线 \_ 转义（显示字符本符）\\
<sup>上标</sup>  \\
<sub>下标</sub> 

```
**加粗**  
_倾斜_   
~~删除线~~   
转义字符，在特殊字符前添加下划线 \_ 转义（显示字符本符）
<sup>上标</sup>  
<sub>下标</sub> 
换行符 \\ 注意后面不加空格
```

### 2) 标题等级

## 大级标题
### 中级标题
#### 小级标题

下一行添加下划线标题
---

下一行添加等号标题
===

```
## 大级标题
### 中级标题
#### 小级标题
在文字下一行添加---，===也会分别将文字格式化为不同大小标题
```

### 3) 书写`行内代码`

```
`行内代码` # 使用两个点包括
```

### 4) 引用
> 引用1  
引用2
> > 嵌套引用

```
> 引用1  
引用2
> > 嵌套引用
```

### 5) 代码段

使用\`\`\`和\`\`\`包裹代码块 \\
若需要进行代码高亮，可使用\`\`\` + 空格 + 语言名 (e.g. html) \\
或者{% raw %}{% highlight language %}及{% endhighlight %}来包裹{% endraw %}

### 6) 无需列表
+ 使用+号书写无序列表
- 使用-号书写无序列表
* 使用\*号书写无序列表

```
+ 使用+号书写无序列表
- 使用-号书写无序列表
* 使用\*号书写无序列表
```

### 7) 有序列表
1. 使用数字+.+空格+内容书写有序列表
1. 具体的数字无需关系（即可以都写1）

```
1. 使用数字+.+空格+内容书写有序列表
1. 具体的数字无需关系（即可以都写1）
```

### 8) 分割线

--- 
可以使用\*\*\*及- - -绘制分割线,注意分割线上方需有空行

*** 


### 9) 超链接
```
[链接显示的名称](链接地址)
```

### 10) 图片插入

可使用如下两种形式，使用第二种可调节图片大小。

``` html
![](图片位置如/blog/image/inPosts/1.png)
<img src="图片位置" width="500" height="100" alt="">
```


### 11) 临时禁止Jekyll tag命令

使用 {% raw %} {% raw/endraw %} {% endraw %} tag对，包裹需要禁止的命令，但似乎是不能对自身使用（即无法嵌套）


