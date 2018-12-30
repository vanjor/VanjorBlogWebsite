---
title: Java界面开发工具WindowBuilder
date: 2010-12-22 23:45:22
updated: 2010-12-22 23:45:22
categories: 知识积累
tags:
  - Java
  - Windows Builder
  - GUI
---
![windows-builder](https://ws2.sinaimg.cn/large/006tNbRwly1fynz8c4114g306s041746.gif)

在项目要进行最终界面展示开发时，又得回到熟悉而久违的的SWT/JFACE，继而追寻那Eclipse的优秀的[WYSIWYG](http://baike.baidu.com/view/6326.htm)插件[WindowBuilder](http://code.google.com/javadevtools/wbpro/index.html)（没用WB之前都是手工敲代码，并界面排版），却惊喜的发现，不用再漫天搜查注册码，如今WindowBuilder的Java/Ajax工具开发商[Instantiations](http://www.instantiations.com/)已经于2010年8月被Google给收购了，WindowBuilder也作为免费工具开放，强大的Google！

并传闻Google这一收购用意不仅在GWT设计工具，更在于Ajax和Java方面，Google所有网页应用都部署了大量的Ajax，而Android应用则使用Java来创建。

<!-- more -->

# WindowBuilder简介

WindowBuilder是一款基于Eclipse平台的双向Java的GUI设计插件式的软件。具备SWT/JFACE开发、Swing开发及GWT 开发三大功能，是一款不可多得的Java体系中的WYSIWYG工具。

WindowBuilder目前最新版是8.1.0

WindowBuilder的主要用户界面构建为：

* **[Design View](http://code.google.com/javadevtools/wbpro/userinterface/design_view.html)** \- the main visual layout area.
* **[Source View](http://code.google.com/javadevtools/wbpro/userinterface/source_view.html)** \- write code and review the generated code
* **[Structure View](http://code.google.com/javadevtools/wbpro/userinterface/structure_view.html)** \- composed of the **Component Tree** and the **Property Pane.**
  * **[Component Tree](http://code.google.com/javadevtools/wbpro/userinterface/component_tree.html)** \- shows the hierarchical relationship between all of the components.
  * **[Property Pane](http://code.google.com/javadevtools/wbpro/userinterface/property_pane.html)** \- displays properties and events of the selected components.
* **[Palette](http://code.google.com/javadevtools/wbpro/userinterface/palette.html)** \- provides quick access to toolkit-specific components.
* **[Toolbar](http://code.google.com/javadevtools/wbpro/userinterface/toolbar.html)** \- provides access to commonly used commands.
* **[Context Menu](http://code.google.com/javadevtools/wbpro/userinterface/context_menu.html)** \- provides access to commonly used commands

# 主要特性

* **[Bi-directional Code Generation](http://code.google.com/javadevtools/wbpro/features/bidirectional.html) \-** read and write almost any format and reverse-engineer most hand-written code
* **[Internationalization (i18n) / Localization](http://code.google.com/javadevtools/wbpro/features/internationalization.html) \-** externalize component strings, create and manage resource bundles.
* **[Custom Composites & Panels](http://code.google.com/javadevtools/wbpro/features/custom_composites.html)** \- create custom, reusable components.
* **[Factories](http://code.google.com/javadevtools/wbpro/features/factories.html)** \- create custom factory classes and methods.
* **[Visual Inheritance](http://code.google.com/javadevtools/wbpro/features/visual_inheritance.html)** \- create visual component hierarchies.
* **[Event Handling](http://code.google.com/javadevtools/wbpro/features/event_handling.html)** \- add event handlers to your components.
* **[Menu Editing](http://code.google.com/javadevtools/wbpro/features/menu_editing.html)** \- visually create and edit menubars, menu items and popup menus.
* **[Morphing](http://code.google.com/javadevtools/wbpro/features/morphing.html)** \- convert one component type into another.

# 基于Eclipse/MyEclipse的安装

目前只能通过windowbuilder更新地址在线安装：  

* [Eclipse 3.6 (Helios)](http://dl.google.com/eclipse/inst/d2wbpro/latest/3.6)  
* [Eclipse 3.5 (Galileo)](http://dl.google.com/eclipse/inst/d2wbpro/latest/3.5)  
* [Eclipse 3.4 (Ganymede)](http://dl.google.com/eclipse/inst/d2wbpro/latest/3.4)

具体详见：[http://code.google.com/javadevtools/download-wbpro.html](http://code.google.com/javadevtools/download-wbpro.html)

# Window-builder开发介绍文档

官方在线文档地址：[http://code.google.com/javadevtools/wbpro/index.html](http://code.google.com/javadevtools/wbpro/index.html)

# 附

使用window-builder快速开发的实验展示demo，熟练的话，差不多半个小时就可以完成。

![window-builder demo](https://ws2.sinaimg.cn/large/006tNbRwly1fynzc4xns3j30ec08n74z.jpg)