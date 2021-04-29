---
layout: post
title:  "Jenkins使用笔记"
date:   2021-04-27 16:05:00 +0100
categories: tool jenkins
---

## 内容提要
- [安装](#安装)
- [构建配置](#构建配置)
	- [执行windows批处理命令](#执行windows批处理命令)
- [构建触发配置](#构建触发配置)
	- [GitHub提交操作触发构建](#github提交操作触发构建)
- [构建后操作配置](#构建后操作配置)
	- [成品归档](#成品归档)
	- [邮件通知](#邮件通知)
- [问题解决](#问题解决)
	- [Jenkins触发的自动化脚本被拒绝访问](#jenkins触发的自动化脚本被拒绝访问)

---

## 安装

Jenkins支持各类操作系统及安装方法，详见官网帮助[Installing Jenkins](https://www.jenkins.io/doc/book/installing/)

## 构建配置

### 执行windows批处理命令

使用该构建步骤，可使得目标任务被触发构建时，执行所配置的批处理命令。

在目标任务的“构建”配置中，选择“执行 Windows 批处理命令”，如下。

<img src="/blog/image/inPosts/2021-04-27-1.png" width="300" alt="">

在出现的命令框中，填入所需执行的命令即可，如下。

![](/blog/image/inPosts/2021-04-27-2.png)

在非Windows系统中，可使用“执行 Shell”选项。

## 构建触发配置

### GitHub提交操作触发构建

可实现每次向GitHub提交代码后，触发目标任务的自动构建。配置步骤如下。

#### S1: 在GitHub账户配置中设置“Personal access token”

登入GitHub账户，点击右上角的头像图标，在弹出菜单中，依次选择“Settings” -> “Developer Settings” -> “Personal access token”。点击“Generate New Token”以创建一个新的token。

在创建token界面中填入token备注，并授权“repo”以及“admin:repo_hook”两类作用域。如下图。

<img src="/blog/image/inPosts/2021-04-27-3.png" width="500" alt="">

点击页面最后的“Generate Token”后，即可生成token。如下图。请记住该串字符，之后将不再显示。

![](/blog/image/inPosts/2021-04-27-4.png)

#### S2: 在Jenkins系统配置中配置“GitHub Server”

在Jenkins主界面中依次点击“系统管理” -> “系统配置”，进入系统配置界面，并在“GitHub”部分配置GitHub服务器，以支持使用GitHub触发。

首先，需添加一个新的GitHub服务器，如下。

<img src="/blog/image/inPosts/2021-04-27-5.png" width="350" alt="">

在出现的配置框内填写所需的名称，并添加一个新的Jenkins凭证。凭证的配置界面如下图，选择的凭证类型为“Secret Text”。在ID及描述框中填入任意的用于标识的内容。

![](/blog/image/inPosts/2021-04-27-6.png)

上述配置完成后，点击“GitHub服务器”配置区域中的“测试连接”按钮，若配置正确，则显示如下图；若有错误，测试将报错。

<img src="/blog/image/inPosts/2021-04-27-7.png" width="500" alt="">

#### S3: 在目标Github仓库中添加一个Webhook
在目标GitHub仓库的“Settings” -> “Webhook”处，添加一个新的Webhook。在其中的“Payload URL”处填Jenkins服务器的地址 + “/github-webhook/”，如图。请注意，该地址需要能够被外部访问。

![](/blog/image/inPosts/2021-04-27-8.png)

若暂无外部可访问地址，请参考S6，其使用ngrok工具映射了一个外网地址，以实现本地的触发测试。

#### S4: 在目标项目中配置“源码管理”

在目标Jenkins任务配置的“源码管理”处，选择“Git”，并按要求填写。如下图。

![](/blog/image/inPosts/2021-04-27-9.png)

其中，在添加仓库凭证时，使用“Username with password”类型，并在对应编辑框中填入使用者GitHub的用户名及密码。凭证配置如下图。

![](/blog/image/inPosts/2021-04-27-10.png)

#### S5: 在目标项目中配置“构建触发器”

在目标Jenkins任务配置的“构建触发器”处，选择“GitHub hook trigger for GITScm polling”，如下图。

![](/blog/image/inPosts/2021-04-27-11.png)

#### S6: 使用ngrok工具实现本地测试

使用ngrok工具，将localhost映射至一个外部地址，以实现GitHub提交触发的本地测试，步骤如下：

1）至[ngrok官网](https://ngrok.com)下载工具。

2）注册一个ngrok账户，并进入账户界面。

3）按照账户界面中的“Getting Started” -> "Setup & Installation"指南进行操作。其主要步骤包括连接账户及实现映射，正确运行映射的界面如下图。

![](/blog/image/inPosts/2021-04-27-12.png)

## 构建后操作配置

### 成品归档

在目标任务的“构建后操作”中，添加一项新的“成品归档”操作，并在其中填入所需的归档条件，如下图。

![](/blog/image/inPosts/2021-04-27-13.png)

### 邮件通知

配置邮件通知时，首先需要在Jenkins的“系统管理” -> “系统配置”中配置“Jenkins Location”、“Extended E-mail Notification”及“邮件通知”，然后，在目标任务的配置中添加一个“Editable E-mail Notification”的构建后操作。以下以qq邮箱作为管理员邮箱为例，详述具体配置步骤。

#### S1: 配置系统管理员邮件地址

在“系统配置”的“Jenkins Location”中填写一个系统管理员的邮件地址，如下图。

![](/blog/image/inPosts/2021-04-27-14.png)

#### S2: 配置“Extended E-mail Notification”

在“系统配置”的“Extended E-mail Notification”处配置发送邮箱相关参数，点击该处的“高级”按钮后，会显示出全部的需配置项，包括SMTP服务器、SMTP端口、SMTP用户名及密码，如下图。其中，SMTP用户名为管理员邮箱地址，密码为管理员邮箱启用STMP时生成的密码，而非邮箱的登录密码。

![](/blog/image/inPosts/2021-04-27-15.png)

可在此处设置通知邮件的内容，包括默认主题、默认邮件内容，接受者列表等，参数说明可通过点击每一个参数配置框最右侧的问号图标显示。

点击“Default Triggers”按钮后，可配置触发邮件通知的选项，如每次构建都发送通知、或是只在构建失败后发送通知等。

#### S3: 配置“邮件通知”

在“邮件通知”处配置通知邮件发送服务器相关的内容，包括SMTP服务器、SMTP端口、SMTP用户名及密码，如下图。其中，SMTP用户名为管理员邮箱地址，密码为管理员邮箱启用STMP时生成的密码，而非邮箱的登录密码。

![](/blog/image/inPosts/2021-04-27-16.png)

可在该配置处的最后勾选“通过发送测试邮件测试配置”，填入目标测试邮件后，可立刻检查邮件通知是否配置正确。若正确，目标测试邮箱会成功收到一封测试邮件；若有误，会在此处显示错误信息。

#### S4: 配置构建后触发操作

在目标任务的“构建后操作”中，添加一项新的“Editable Email Notification”操作。使用“Email Notification”操作也可，但其可配置项较“Editable Email Notification”相比较少，“Editable Email Notification”可以更灵活地按需配置发送邮件的内容，如邮件主题、内容、目标接受者等。具体每一个配置项的说明，可在点击各配置框右侧的问号按钮后显示。

## 问题解决

### Jenkins触发的自动化脚本被拒绝访问

#### 问题描述

在Jenkins目标任务中，添加一项执行Windows批处理操作的构建步骤。通过命令执行一个python脚本，用于自动化运行Vector CANoe软件。批处理命令如下。
```
cd c:\Users\the\target\folder
python auto.py
```
该python脚本auto.py在本地直接运行可正常工作，而通过Jenkins触发执行时报错，被拒绝访问。

#### 问题解决

需配置相关服务的账户权限，主要步骤如下。

1）打开dcomcnfg.exe，Service中找到Jenkines服务，在其Properties中配置Log On账户，如下图。

![](/blog/image/inPosts/2021-04-27-17.png)

2）在同一界面的Component Services -> Computers -> My Computer -> DCOM Config中找到Vecter CANoe Application，并在属性->security->launch and activation permission 中添加对应账户的权限，如下图。

![](/blog/image/inPosts/2021-04-27-16.png)

完成上述配置后，即可实现通过Jenkins运行脚本后，调用CANoe自动化接口。

## Reference
- [GitHub Push触发Jenkins自动构建](https://cloud.tencent.com/developer/article/1704831)
- [Jenkins qq邮箱配置实例演示](https://blog.csdn.net/qq_38161040/article/details/100886849)