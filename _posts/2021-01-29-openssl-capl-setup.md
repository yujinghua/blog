---
layout: post
title:  "CANoe中调用OpenSSL库函数(CAPL DLL开发)"
date:   2021-01-29 21:55:00 +0100
categories: tool openssl
---

## 内容提要
- [VS console app project中使用OpenSSL库函数](#vs-console-app-project中使用openssl库函数)
- [创建CAPL DLL并配置CANoe调用环境](#创建capl-dll并配置canoe调用环境)
- [CAPL DLL接口编写](#capl-dll接口编写)

---

## VS console app project中使用OpenSSL库函数

创建独立的console app project，是为了在VS环境中能便捷地对OpenSSL库函数进行学习及测试。对所需使用的算法完成编写及简单功能测试后，再移入需要被CANoe调用的dll文件中，以免去联合CANoe环境调试DLL地不便。

1) OpenSSL库安装

方法 1: 使用 [安装包](http://slproweb.com/products/Win32OpenSSL.html) \\
方法 2: 从官网 [OpenSSL website - download](https://www.openssl.org/source/) 下载源码并本地编译。

2) 将OpenSSL的.dll及.lib文件路径添加至vs project配置中的include directiories及library directiories处，如下图（Project -> 项目名 Property）。OpenSSL的include及lib文件的目录在OpenSSL的安装目录下。

![](/blog/image/inPosts/2021-01-27-1.png)

3) 将OpenSSL库名称(libcrypto.lib及libssl.lib)添加至vs project配置的addtional dependencies中，如下图（Project -> 项目名 Property）。

![](/blog/image/inPosts/2021-01-27-2.png)

4) include所需函数对应的.h文件后，即可使用相应的库函数。

## 创建CAPL DLL并配置CANoe调用环境

1) Vector提供了CAPL DLL的模板文件（e.g. C:\Users\Public\Documents\Vector\CANoe\Sample Configurations 13.0.118\Programming\CAPLdll\，可在CANoe的File->Sample Configuration -> programming -> CAPL DLL 的项目详情中查看路径），将该模板文件拷贝到需要的路径下，并更改.vcxproj文件名为需要的名字（非.sln文件名），双击.vcxproj打开工程。

2) 在project配置中（Project -> 项目名 Property），修改需要输出的DLL文件路径及名称，如下图。

![](/blog/image/inPosts/2021-01-27-3.png)

3) 为了调用OpenSSL， 需要在project配置中添加include及lib文件的路径，以及依赖的库名称。同本文第1部分的2),3)步骤。

4) 完成配置后，即可在该DLL项目中调用OpenSSL的函数(记得include所使用函数的.h文件)。使用Build->Build Solution(ctrl+B)编译后，即可在目标文件夹生成DLL文件。

5) 在CAPL中使用时，需同时include自制的能被CAPL识别的dll（即使用CAPL DLL模板修改而得的DLL）以及OpenSSL的dll文件。调用方法有两种：

a) 在.can文件中的includes下包含需调用DLL文件的路径，如下。

```
includes{
    #pragma library("C:\Program Files (x86)\OpenSSL-Win32\libcrypto-1_1.dll")
    #pragma library("C:\Program Files (x86)\OpenSSL-Win32\libssl-1_1.dll")
    #pragma library("C:\Users\your\dll\path\OpenSSL_CAPL.dll")
}
```

b) 或在CANoe环境中的 File -> Option 配置界面中添加CAPL dll的路径，如下。

![](/blog/image/inPosts/2021-01-27-4.png)

6) 完成上述配置后，即可在CAPL环境中调用在DLL中设计的函数了。

## CAPL DLL接口编写

CAPL DLL的接口编写主要在VS DLL工程的capldll.cpp文件中进行。以下以一个求和函数为例，介绍如何编写一个能够被CANoe环境调用的函数接口。

1) 定义函数。确定函数名（任意，非调用接口名），参数及具体的函数执行代码，如下。

``` c
int32_t CAPLEXPORT CAPLPASCAL appAdd(int32_t x, int32_t y)
{
  int32_t z = x + y;
  return z;
}
```

2) 在同一个文件中的`CAPL_DLL_INFO4 table[]`数组内（该数组为CAPL Export Table)，根据定义的规则，添加一行本函数的信息，如下。

```c
{"dllAdd", (CAPL_FARCALL)appAdd, "CAPL_DLL", "This function will add two values. The return value is the result",'L', 2, "LL", "", {"x","y"}},

```

3)在编译完成并生成新的DLL文件后，该新增函数即可在CAPL环境中正常使用。CAPL中显示的函数名为CAPL Export Table中定义的名称，如本例中的`dllAdd`。其他函数相关信息（如：分组，描述，参数等）均会在CAPL编辑器的函数帮助信息中显示。

### Reference

CAPL Export Table的定义具体参考CANoe help中的“Description of the CAPL Export Table”（CAPL » CAPL DLL » Export Table）文档。

## Notes

* 使用的软件版本如下: Visual Studio Community 2019; OpenSSL 1.1.1h; CANoe 13.0 SP1

* 本文可作为一般性的CAPL DLL开发参考

