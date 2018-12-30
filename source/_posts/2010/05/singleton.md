---
title: 设计模式-单例模式
date: 2010-05-23 13:19:50
updated: 2010-05-23 13:19:50
categories: 知识积累
tags:
  - Desgin Pattern
  - Java
---

**Java Design Pattern - Singleton（单例模式）**:

<!-- more -->

# 模式概述

* 归类：责任型模式
* 目标：将责任集中到某个类的单个实例上，确保某个类只有一个实例，并且为其提供一个全局访问点
* 实现方法：通过隐藏构造器和提供对创建对象的单个访问点，将类的职责集中于类的某单个实例中

样列：UdpPortManager

```Java
public class UdpPortManager{

    private static UdpPortManager udpPortManager;

    // 构造器私有化，对外不可见
    private UdpPortManager() {
    }

    /*
    * 提供同步，线程安全的全局访问点
    */
    synchronized public static UdpPortManager getInstance(){
        if(udpPortManager==null){
            udpPortManager=new UdpPortManager();
        }
        return udpPortManager;
    }

    public void method(int port) {
        //自定义方法
    }
}
```

注意:

* 所包含实例变量应该是静态的私有的
* 外部调用该实例时，不是通过类的构造方法，而是通过一个getInstance()这样的静态方法来创建并获取该类的唯一实例。
* 默认的构造方法应该是私有的，这样保证了无法从外界创建该类的新实例
* 单例模式并不是线程安全的，同步关键字synchronized应用在getInstance保证只实例化一个

# 模式扩展

单列模式细分可以分为：懒汉式单例、饿汉式单例、登记式单例三种。参考：[franksinger](http://franksinger.javaeye.com/blog/531658)

* 懒汉单列：第一次调用getInstance()方法才初始化实例，一章节中的样列属于此种情形。
* 饿汉单列：静态直接初始化，private static UdpPortManager udpPortManager = new UdpPortManager();及在类被JVM加载时就初始化单列实例，而不必等调用getInstance()方法。
* 登记式单列：实际上接近多列模式了，就是类内部维护多个实例，外部调用共享改类所持有的实例。样列详见上述所给链接。Java中的Enum类实际上就是单列模式的一种扩展，Enum类中持有多个实例。