---
layout: post
title:  "软件设计课程 培训记录"
categories: design
date: +0800 2017-09-01 08:13:05
tag: design pattern 培训
summary: "<p>所有软件设计必须拥抱变化，适应变化。因为除了根本不用的系统，所有的软件系统不可能是需求不变的。</p>
<p>大多数老系统都具有代码烂，维护困难的特点。我们在软件设计初期和维护时期都可以使用一些方法来防止出现这类问题...</p>"
---

* content
{:toc}

# 主要内容
--------------
- 软件生命周期内遇到的问题
- 解决软件痛点的方法
- 讲解软件设计基本原则
- 软件设计模式


# 软件生命周期内遇到的问题
------------------

## 需求是不定的
-----------------
**“通常，客户不知道他们需要的是什么，除非我们秀出了产品。”**

所有软件设计必须拥抱变化，适应变化。因为除了根本不用的系统，所有的软件系统不可能是需求不变的。

大多数老系统都具有代码烂，维护困难的特点。我们在软件设计初期和维护时期都可以使用一些方法来防止出现这类问题。

## 老代码维护困难
-----------------
很多时候，由于各种原因，导致一个系统的代码越来越烂，以至于几乎无法维护。

软件的修改有一种障碍成本。障碍成本包括：
1. 理解成本

    修改代码需要理解老的代码，理解过程是一个很耗时的过程。

1. 修改成本

    修改成本一般较小，除非代码过滥，太多重复，此时才需要修改大量代码。

1. 测试成本

    测试成本一般是较大的，改动一处对测试的成本就有一些了，如果改动一堆则测试成本会非常大

1. 发布成本

    发布成本一般也比较大，因为发布功能可能要停服务，需要走非常多的复杂流程。

比如修改所有异常处理的方法，加一个“发短信”功能，如果之前都没有一个统一的异常处理方法，则要改死人了。


# 面向对象程序设计原则
-------------
这里“设计原则” 指面向对象编程和面向对象设计的五个基本原则。
当这些原则被一起应用时，它们使得一个程序员开发一个容易进行软件维护和扩展的系统变得更加可能。

## 开闭原则
-------------
**“软件中的对象（类，模块，函数等等）应该对于扩展是开放的，但是对于修改是封闭的.”**

以电脑主板为例，我们可以很容易地修改容易变化的部分，包括CPU、内存、声卡显卡、外设等。这体现“对扩展开放”的原则。
而南北桥芯片、各种电容等东西则是封闭的，因为它们**不常被修改**，并且实现可修改的难度很大，所以焊接在主板上。

## 单一职责原则
------------------
**“一个类或者模块应该有且只有一个改变的原因.”**

例如一个认证类，有验证和保存两个功能，参数为用户名和密码。现有的Demo实现是：
```java
public class Authenticator {
    final public void save(String user, String password) {
        // 加密并保存
    }

    final public boolean authenticate(String user, String passwrod) {
        // 加密并验证
    }

    void store(St ring, user, String encryptedPassword) {
        // 保存
    }

    String retrieve(String user) {
        // 获取密文密码
    }

    String encrypt(String text) {
        // 加密
    }
}
```
后来在正式开发后，加密方法发生了变化，有客户需要AES、DES等；存储方法也有变化，有文件、消息队列、数据库等。

此时我们想要将`store`、`retrieve`、`encrypt`方法拆分成接口类。

如果简单来想，把这三个功能都拎出来作为一个接口来实现方法，则会发生如下问题：
- 如果有5种加密模式，5种存储模式，则需要25个类来实现

所以我们不能这样来拆分。拆分要根据**变化的因素**来决定。所以这里有一种拆分方法的分析：
- **变化**的是“加密方法”与“存储位置“，所以拆分成”加密接口“与”存储接口“

此时两个接口分别解决了加密方法与存储未知的变化。

如果之后需要使存取方法不同，则”存取方法”也是一个**变化**，则需要将存取接口拆分为两个。

## 里氏替换原则
---------------
**“派生类（子类）对象能够替换其基类（超类）对象被使用。”**

## 接口隔离原则
--------------
**“没有客户（client）应该被迫依赖于它不使用方法。”**

不要让客户端知道太多不给他用的接口。

## 依赖倒转原则
---------------
依赖倒转原则是一种解耦原则，规定：
1. 高层模块不应该直接依赖底层模块，而依赖于**接口**
1. **接口**不应依赖于实现，而实现要依赖于接口

以主板为例，我们不能让主板只适应某种品牌的内存、硬盘，而要依赖其接口定义，DDR4、SATA等；
而接口的定义更不能随着具体实现的变化而变化。

## 其他设计原则
--------------------
1. 尽量使用合成和聚合，而不是继承来达到复用的目的。

    因为继承是依赖关系很严密的方法。

1. 动态绑定

    提倡运行时的动态绑定方法来解决变化，而不是编译期决定。
    比如蜡笔有3种粗细，12种颜色，则有36个蜡笔；而画笔只需3种画笔12中颜色蘸着用即可

1. 创建与使用分离

    要么创建，要么使用，不要在同一处逻辑里两者兼得

1. 最少知道原则

    我们买东西总不会把自己的钱包给收银员让他去扣款


# 软件设计模式
---------------
设计模式是对软件设计中普遍存在（反复出现）的各种问题，所提出的解决方案。

《设计模式》中提及23种设计模式，分为创建类模式、结构类模式、行为类模式。

这里介绍一些印象深刻的设计模式，系统地学习需要参考书籍。

## 创建型模式
--------------
这些设计模式提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 运算符直接实例化对象。
使得程序在判断针对某个给定实例需要创建哪些对象时更加灵活。

创建型模式包括：
- 工厂模式（Factory Pattern）
- 抽象工厂模式（Abstract Factory Pattern）
- 单例模式（Singleton Pattern）
- 建造者模式（Builder Pattern）
- 原型模式（Prototype Pattern）

例如Gson利用GsonBuilder的创建过程，就是一种典型的建造者模式，用于适应各式各样的场景。
```java
Gson gson = new GsonBuilder().addSerializationExclusionStrategy(new ExclusionStrategy() {

    @Override
    public boolean shouldSkipField(FieldAttributes f) {
        if ("specialFlag".equals(f.getName())) {
            return true;
        }
        return false;
    }

    @Override
    public boolean shouldSkipClass(Class<?> clazz) {
        return false;
    }
}).create();
```

## 结构型模式
--------------
这些设计模式关注类和对象的组合关系。

结构型模式包括：
- 适配器模式（Adapter Pattern）
- 桥接模式（Bridge Pattern）
- 过滤器模式（Filter、Criteria Pattern）
- 组合模式（Composite Pattern）
- 装饰器模式（Decorator Pattern）
- 外观模式（Facade Pattern）
- 享元模式（Flyweight Pattern）
- 代理模式（Proxy Pattern）



## 行为型模式
--------------
这些设计模式特别关注对象之间的通信。

行为型模式包括：
- 责任链模式（Chain of Responsibility Pattern）
- 命令模式（Command Pattern）
- 解释器模式（Interpreter Pattern）
- 迭代器模式（Iterator Pattern）
- 中介者模式（Mediator Pattern）
- 备忘录模式（Memento Pattern）
- 观察者模式（Observer Pattern）
- 状态模式（State Pattern）
- 空对象模式（Null Object Pattern）
- 策略模式（Strategy Pattern）
- 模板模式（Template Pattern）
- 访问者模式（Visitor Pattern）

# 知识应用
--------------

## 设计题目
--------------
利用设计原则与设计模式，设计如下系统。

### 日志系统

现在我们需要一套日志系统，需要几个阶段的不同设计：
1. 打印到Console即可
1. 要求可以输出到Console也可以输出到文件或Socket
1. 要求输出中带有时间、线程编号等信息
1. 要求不同的日志有不同的格式，XML、JSON、TXT、自定义格式
1. 要求满足某种过滤条件才输出到指定输出位置与格式

### 统一异常处理组件

为了方便对各种异常的处理更灵活，防止好多cp代码，要求一个统一异常处理组件，需求：
1. 实现统一的异常接口，可配置可扩展
1. 实现默认的异常处理方法
1. 可以配置多个处理方法，包括日志、短信、邮件等
1. 可以决定是否继续抛出异常，新异常可能和之前的不是同类异常

### 餐馆

据说一般的餐馆都用了17种左右的设计模式来设计其业务流程…

## 拒绝软件退化
--------------
作为程序员，首先要声明：*“我在修改代码时，我会更加小心，不会让其变得更糟”*。

代码变烂的原因往往不是外部因素，而是程序员自身的问题：
1. 根本不考虑自己所写的代码的后果
1. 对设计模式与原则不熟悉
1. 由于原代码本身的复杂性，不敢动

修改的代码要**尽量不修改原来的代码**，**修改范围越小越好**，**要方便接下来的测试**。

加新功能的方法大致有：
1. 加新方法
1. 加新类
1. 外覆方法

    比如把原来的函数重命名，加一个新函数使用与原函数有相同的签名

1. 外覆类

    集成一个类，使用super方法调用原方法

## 一般的公共组件
--------------
据说以Oracle公司的建议，一般有如下类型的公共组件：
- Caching Service           公共缓存
- Configuration Service     配置服务
- MessageQueue Service      消息队列
- EventNotification Service 事件服务
- Authorization Service & Authentication Service    授权与认证服务
- Transaction Service       事务服务
- Exception Service         异常处理服务
- Cryptography Service      安全服务
- Workflow Service          工作流系统
- Time Service              定时任务调度系统
- Logging and Instrumentation   日志与监控
- State Management          状态管理
- Validation                验证方法
- Communication             通信机制


# 推荐书籍
--------------
- 《设计模式:可复用面向对象软件的基础》
- 《面向模式的软件体系结构》
- 《修改代码的艺术》
- 《程序员的自我修养》
