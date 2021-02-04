---
layout: post
title:  "(非官方)同济大学硕博论文Letax模板使用笔记"
date:   2021-02-04 20:53:00 +0100
categories: acad tongji
---

## 内容概述

本文主要为使用[marquistj13/TongjiThesis](https://github.com/marquistj13/TongjiThesis)论文模板的应用笔记，主要内容包括：

1. 在texlive 2020，MAC环境下的模板文件使用步骤。由于在实测中，与原版[readme.md](https://github.com/marquistj13/TongjiThesis/blob/master/README.md)中的说明略有不同，故更新说明。
1. 消除编译后warning的方法。
1. 对比官方的格式要求，修正了原版中页面设置的不符之处。

---

## 官方要求
当前学校的官方要求及word模板详见：[同济大学学位论文写作规范(2019.4更新)](https://gs.tongji.edu.cn/info/1063/1754.htm)

## 模板文件使用步骤

### 安装环境texlive

本文使用了texlive 2020，在[The MacTeX-2020 Distribution](https://tug.org/mactex/)页面中点击 MacTeX Download 下载.pkg包。下载后根据安装包提示使用即可。

完成安装后，用TEXstudio打开主tex文件(thesis.tex)即可进行文件构建。（当然，实际使用时大概率此时编译会报错，因为会缺少所需字体。因此，需要安装字体，详见下一小节“安装字体”）

### 安装字体
本运行环境中缺少隶书字体，根据原版[readme.md](https://github.com/marquistj13/TongjiThesis/blob/master/README.md)中说明，MAC系统的隶书配置，除了在系统中安装字体外，还需手动进行一些配置。由于本人在实践中与原作者情况略有不同，故记录如下。

1）在[字体安装](https://github.com/marquistj13/TongjiThesis/issues/18)处下载原项目中提供的字体，双击“LiShu.ttf”文件，点击“安装字体”后即可进行安装。

2）在“字体册.app”中可以查看到已安装的“隶书”字体。点击该窗口左上角的“i”图标，查看该字体的PostScript名称为“LiSu”。

3）打开在 /usr/local/texlive/2020/texmf-dist/tex/latex/ctex/fontset/ctex-fontset-macnew.def，在文件中相似定义的地方后面添加如下两行，以定义lishu命令。

```
\setCJKfamilyfont { zhli } { LiSu } # 约line 57

\NewDocumentCommand \lishu { } { \CJKfamily { zhli } } # 约line 66
```

4）在主tex文件thesis.tex中添加如下配置。
* 在line 1的\documentclass中添加fontset = none配置，添加后为`\documentclass[fontset = none, degree = doctor, bibtype = numeric]{tongjithesis}`
* 在\documentclass下一行添加`\ctexset{fontset = mac}`, 用来自定义使用的字符集为mac，因为经测试本系统自动会使用ctex-fontset-fandol字符集。关于字符集的具体描述，可参考[CTEX 宏集手册](https://ctan.mirror.norbert-ruehl.de/language/chinese/ctex/ctex.pdf)中4.3章节。
* 完成缺失字体的安装后，即可实现正常排版编译。
* 完整ctex包信息详见[ctex – LATEX classes and packages for Chinese typesetting](https://www.ctan.org/pkg/ctex)。

## 消除warning
虽然有些报出的warning并不影响排版，但强迫症们可以按照以下方法消除与排版相关的warning。和具体章节内容的warning，可以根据提示进行内容修改。

1. Package hyperref Warning: Option ‘pdfpagelabels’ has already been used,(hyperref) setting the option has no effect on input line 213.

	去除方法：在tongjithesis.cls中的约line 212, 注释掉pdfpagelabels

1. Package hyperref Warning: Token not allowed in a PDF string (Unicode):(hyperref) removing '\\\\' on input line 34.

	去除方法：去掉cover.tex文件中标题中换行符 \\\\，警告即会消失。（[参考讨论](https://m.newsmth.net/article/TeX/323639)）

1. Package fontspec Warning: Font "Songti SC Light" does not contain requested(fontspec) Script "CJK". 等一系列类似warning。

	出现该警告是因为字体文件中不包含要求的内容（字体文件的问题），但不影响显示，可以抑制fontspec警告。([参考](https://zhuanlan.zhihu.com/p/145429470))

	去除方法：在tongjithesis.cls中的约line 108（原\PassOptionsToPackage行后面）添加\PassOptionsToPackage{quiet}{fontspec}，将warning写入log中，而不报warning；若使用PassOptionsToPackage{silent}{fontspec}，则这些warning连log文件中都不会被写入。（参考[The fontspec package文档](https://mirror.dogado.de/tex-archive/macros/unicodetex/latex/fontspec/fontspec.pdf)3.4章节，fontspec信息详见[fontspec – Advanced font selection in XELATEX and LuaLATEX](https://www.ctan.org/pkg/fontspec))

1. Package Fancyhdr Warning: \footskip is too small (15.36446pt):Make it at least 15.61334pt. 

	该warning是指所设置的footskip太小了。根据官方要求，应该设置的footskip应为25.4 - 20 = 5.24mm。但根据warning提示该值最小应为设置为15.61334pt（即5.51mm）。（暂未深究限制原因-todo）为了消除warning，还一个清净的工作环境，撰写时可调成5.51mm，排版效果差别很细微。若想要严格按照官方设置要求，可最后再改成严格要求的5.4mm即可。

## 页面设置参数修正

### 修改\geometry命令中的参数

下图为官方要求的页面参数。geometry包中对参数的说明示意图详见[The geometry package](http://ctan.ebinger.cc/tex-archive/macros/latex/contrib/geometry/geometry.pdf)中的Figure 1。
![](/blog/image/inPosts/2021-02-04-1.png)

由此在\geometry中填入的参数如下（与原版不同的修正值在注释中已标出）。

```
\geometry{
    a4paper, % 210 * 297mm
    hcentering, %将hmarginratio设为1:1，即left=right
    ignoreall, %body部分不包含top,bottom,left,right，即textheight就是正文的高度。
    textheight = 241.2mm,% 修正值: 297 - 25.4 x 2 - 5(headsep) = 241.2
    bottom = 25.4mm,
    nomarginpar,% 即\marginparwidth = 0pt and \marginparsep = 0pt
    left = 31.7mm, % 注意left = right
    headheight = 5mm, % 增加解释: 与页眉字体大小无关
    headsep = 5mm, % 修正解释:为了满足正文段前首行0.7(14bp)倍行的要求(在官方的word模板中提及)
    footskip = 5.51mm}    %\ 增加解释，该值应为25.4 - 20 = 5.4，设置为5.51是为消除warning信息（上一小节中已说明)
```
### 修改各章标题前后的段距

官方要求如下，对于各章的标题，需要段前距为24bp。
![](/blog/image/inPosts/2021-02-04-3.png)
但在实测中，发现对应格式代码处的配置无效。在参考了[设置每章的标题的上下间距在哪里可以调整?](https://github.com/ustctug/ustcthesis/issues/102)该问题讨论后，发现配置无效的原因为：章标题在页首时，beforeskip自动忽略。解决方案为：在\ctexset命令chapter中的最后添加了fixskip=true配置，即可实现需要的间距，整体如下。

```
\ctexset{ 
  chapter={ %章标题
    ... 省略 ...
    beforeskip = {10bp}, % 段前距设置
    afterskip = {18bp}, % 段后距设置
    ... 省略 ...
    fixskip=true % 添加配置
  },
  ... 省略 ...
}
```

其中，beforeskip为标题的段前距，应设置的值为24bp（要求值）- 14bp（之前设置的headskip值）= 10bp。

### 页面设置调整后实际效果

1磅 = 0.03527cm
 
<img src="/blog/image/inPosts/2021-02-04-4.png" width="400" height="" alt="">

<img src="/blog/image/inPosts/2021-02-04-5.png" width="400" height="" alt="">

<img src="/blog/image/inPosts/2021-02-04-6.png" width="200" height="" alt="">

#### 根据上述修改完成后的文件，可直接在[此处](https://github.com/yujinghua/TongjiThesisTemplate)处获取。

关于页边距的问题，在原项目issue中也有讨论，详见[关于页边距](https://github.com/marquistj13/TongjiThesis/issues/23)。可能是由于运行环境的不同，排版结果会有出入，实际使用中还需根据自己的运行环境测试检查，以符合官方要求。

#### 注：以上如有误，请联系指正。


### 关于许可证书

原项目并未添加license说明，已题issue，待回复 [todo]。

## 感谢 

本项目的contributors: \\
[marquistj13](https://github.com/marquistj13) \\
[CNchence](https://github.com/CNchence) \\
[Williamwenda](https://github.com/Williamwenda) \\
[shoujiaxin](https://github.com/shoujiaxin)  

## 前人种树 后人乘凉

