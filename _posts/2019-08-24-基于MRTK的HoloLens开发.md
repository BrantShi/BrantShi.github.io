---
layout: post
title: "基于MRTK的HoloLens开发（1）"
date: 2019-08-24
excerpt: "使用MixedRealityToolKit进行HoloLens的应用开发"
tags: [unity, MRTK, Hololens]
comments: true
---
* TOC
{:toc}
<figure>
	<img src="http://brantshi.github.io/assets/img/post_img/photo01.jpg">
	<figcaption>MR眼镜Hololens——将科幻变成现实</figcaption>
</figure>

#  Hololens开发

笔者的团队近日有幸入手一台Hololens真机，进行某知名公司一建筑模型借助Hololens的动态展示的开发。开发过程中因国内进行相关开发的团队过少，且微软官方已将原HoloToolKit改版为MixedRealityToolKit也即MRTK，能搜到的有关Hololens开发的博客很少，且大都缺乏时效性，对现今开发Hololens的团队来说指导作用不大；而官方文档又只有英文版且对入门者不太友好，总之开发过程中困难重重。
因而决定写这篇文章，为==拥有Hololens真机，使用最新版本MRTK==进行Hololens项目开发人员提供帮助。
#  环境配置
以下均为笔者所用配置环境
名称     | 环境
-------- | -----
操作系统  | Windows 10 1809【至少1703+】
Visual Studio  | Visual Studio Community 2019
Unity  | Unity 2018.4.6f1【至少2018.4】
## Visual Studio具体配置
Visual Studio的配置是否正确决定了项目能否成功从Unity中构建导出，进而设计能否在Hololens生成APP及调试。下图为笔者经多次尝试后确认的正确配置所需选择的。
![VS具体配置1](https://img-blog.csdnimg.cn/20190823182200401.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)
![VS具体配置2](https://img-blog.csdnimg.cn/2019082318224664.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)
![VS具体配置3](https://img-blog.csdnimg.cn/20190823182429271.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)
![VS具体配置4](https://img-blog.csdnimg.cn/20190823182500836.png)
##  MRTK 工具包配置
在进行Hololens的开发时，我们需要集成微软官方提供的 MixedRealityToolKit 项目。MixedRealityToolKit ，即原HoloToolkit-Unity 项目，简称MRTK，是微软官方的开源项目，用于帮助开发者快速开发 HoloLens 应用，能够快速为项目集成基本输入、空间映射和场景匹配等特性。

关于该项目的详细介绍，可以参考MRTK官方说明文档https://microsoft.github.io/MixedRealityToolkit-Unity/README.html

集成步骤

 - 在Github上下载MRTK项目代码 https://github.com/microsoft/MixedRealityToolkit-Unity
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823183744585.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)
 - 将下载的ZIP解压，使用Unity以打开工程的方式打开解压得到的文件夹，右击Assets，选择Export Package，将所有MRTK前缀的包全部选上，导出得到一unitypackage格式的文件，即是后续在Unity项目中可直接导入的MRTK工具包。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823192416100.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)
## Unity配置
Unity 3D是进行Hololens开发的主要平台，下面进入正题。
配置步骤：
 - 使用Unity新建一个3D项目，由左上角选项栏沿Assets-import package-custom package途径引入上一步中导出的unitypackage文件。
 - 在成功导入后选项框上会出现一个新的选项——Mixed Reality Toolkit，点击并选择Add to Scene and Configure,选择添加图中高亮的MixedRealityToolKitConfigurationProfile，随即左侧框中出现MRTK及MRPlaySpace。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823195120840.png) ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823195214772.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823195311416.png)
 - 由于Hololens内装的是UWP版的Windows 10系统，而Unity默认创建项目运行的平台即标准版本，与之不符，因而需在左上角选项栏中沿File-Build Settings去转换平台为UWP版，相关设置更改如下图，且勿漏选，错选。（笔者使用的是Unity 2018.4.6f1版本）选好后点击Switch Platform即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823195606291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)
 - 仍在Building Settings中点击左下角的Player Settings，在Unity右侧的Inspector中选择XR Settings，勾选其中的Virtual Reality Support 和WSA Holographic Remoting supported。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823195912949.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)
 - 截至此步基本配置已完成，可通过Holographic Remoting Player与设备连接，点击开始即快捷地调试已有项目。注意Holographic Remoting Player是在Hololens上安装，电脑端通过Window-XR-Holographic Emulation途径打开下面的界面，Emulation Mode 选择Remote to Device，在Hololens上打开Holographic Remoting Player后即可获取Hololens的ip地址，输入到Remote Machine中即可。
详细使用方法见：https://docs.microsoft.com/zh-cn/windows/mixed-reality/holographic-remoting-player
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823205828382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)
##  补充
 - 强烈推荐在Windows Store（Windows自带）上下载Microsoft HoloLens，可实时获取设备第一视角的直播，及进行实时照相，录屏等功能，方便团队开发。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823211522910.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823211834466.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JyYW50X1N0YXJr,size_16,color_FFFFFF,t_70)


