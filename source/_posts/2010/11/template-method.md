---
title: 设计模式-模板方法
date: 2010-11-08 16:55:13
updated: 2010-11-08 16:55:13
categories: 知识积累
tags:
  - Design Pattern
  - Java
---

**Java Design Pattern - Template Method ( 模板方法 )**.

<!-- more -->

# 模式概述

* **归类**: 操作性模式
* **目标**: 在一个方法中实现一个算法，把算法中的某些步骤定义进行抽象，推迟到子类中去重新定义，或具体实现。
* **实现方法**: 准备一个抽象类，定义一个操作中的算法的骨架，将一些步聚声明为抽象方法迫使子类去实现。不同的子类可以以不同的方式实现这些抽象方法。
* **样列**: 以[WIKI](http://zh.wikipedia.org/zh/%E6%A8%A1%E6%9D%BF%E6%96%B9%E6%B3%95)中的的样列代码为列

```Java
/**
* An abstract class that is common to several games in
* which players play against the others, but only one is
* playing at a given time.
*/
abstract class Game {

    private int playersCount;
    abstract void initializeGame();
    abstract void makePlay(int player);
    abstract boolean endOfGame();
    abstract void printWinner();

    /* A template method : */
    final void playOneGame(int playersCount) {
    this.playersCount = playersCount;
        initializeGame();
        int j = 0;
        while (!endOfGame()){
            makePlay(j);
            j = (j + 1) % playersCount;
        }
        printWinner();
    }
}

//Now we can extend this class in order to implement actual games:

class Monopoly extends Game {
    /* Implementation of necessary concrete methods */
    void initializeGame() {
        // ...
    }

    void makePlay(int player) {
        // ...
    }

    boolean endOfGame() {
        // ...
    }

    void printWinner() {
        // ...
    }
    /* Specific declarations for the Monopoly game. */
    // ...
}

class Chess extends Game {
    /* Implementation of necessary concrete methods */
    void initializeGame() {
        // ...
    }

    void makePlay(int player) {
        // ...
    }

    boolean endOfGame() {
        // ...
    }

    void printWinner() {
        // ...
    }

    /* Specific declarations for the chess game. */
    // ...
}
```

对于具有公用的程式，通过抽象类Game定义模板方法与对外行为步骤，明确对外行为规范，具体实现子类，ChessGame按照父类接口程式实现各自方法内部逻辑功能。

不同的子类游戏通过共同父类，实现统一接口，消除代码重复，与混乱不一致，并做到代码即是文档接口，方便他人扩展与实现。

模板方法不要求定义子类前编写具体模板方法，而是抽象算法框架，上移至超类，简化和组织代码，作为开发者间的一种约束。

这就是：

通常我们会遇到这样的一个问题：我们知道一个算法所需的关键步聚，并确定了这些步聚的执行顺序。但是某些步聚的具体实现是未知的，或者是某些步聚的实现与具体的环境相关。

模板方法模式把我们不知道具体实现的步聚封装成抽象方法，提供一些按正确顺序调用它们的具体方法(这些具体方法统称为模板方法),这样构成一个抽象基类。子类通过继承这个抽象基类去实现各个步聚的抽象方法，而工作流程却由父类来控制。

**JDK中样列**:

![template method](https://ws3.sinaimg.cn/large/006tNbRwly1fynygm0dzuj30dz053aa1.jpg)

JDK类：Arrays和Collections提供的sort(）方法

# 深入模式

**模式中两种角色**:

* **抽象模版（AbstractClass**: 定义了一个或多个抽象操作，以便让子类实现。这些抽象操作叫做基本操作，它们是一个顶级逻辑的组成步骤。定义并实现了一个模版方法。这个模版方法一般是一个具体方法，它给出了一个顶级逻辑的骨架，而逻辑的组成步骤在相应的抽象操作中，推迟到子类实现。顶级逻辑也有可能调用一些具体方法。
* **具体模版（ConcreteClass）**: 实现父类所定义的一个或多个抽象方法，它们是一个顶级逻辑的组成步骤。每一个抽象模版角色都可以有任意多个具体模版角色与之对应，而每一个具体模版角色都可以给出这些抽象方法（也就是顶级逻辑的组成步骤）的不同实现，从而使得顶级逻辑的实现各不相同。

**适用场景**:

在高级语言中，模板方法通过多态，继承，封装而灵活的应用，总之模板类适用于以下场景：

* **延迟具体实现**: 一次性实现一个算法的不变的部分，并将可变的行为留给子类来实现。
* **公用行为归纳**: 各子类中公共的行为应被提取出来并集中到一个公共父类中以避免代码重复。其实这可以说是一种好的编码习惯了。
* **控制子类扩展**: 模板方法只在特定点调用操作，这样就只允许在这些点进行扩展。如果不愿子类来修改你的模板方法定义的框架，你可以采用两种方式来做：一是在API中不体现出你的模板方法；二将模板方法置为final组织被覆盖。

# 参考

1. Javaeye: [http://www.javaeye.com/topic/78611](http://www.javaeye.com/topic/78611)
2. cnBlog: [http://www.cnblogs.com/zhenyulu/articles/79894.html](http://www.cnblogs.com/zhenyulu/articles/79894.html)