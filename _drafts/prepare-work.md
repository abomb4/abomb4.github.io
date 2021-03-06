---
layout: post
title:  "东方赛算的算带"
categories: 数据结构
date: +0800 2018-02-24 15:13:00
tag: 数据结构
---

* content
{:toc}

# 目标
--------------
复习面试可能遇到的问题。

## 自我介绍
我叫杨睿龙，94年生日，毕业以来一直在恒生电子工作，已经正式工作4年；
期间以 java 后端业务开发为主，在团队更多承担一些偏向技术架构的任务。
最近在做公司规划的融资软件产品，负责整块融资功能的设计与开发。
找工作是为了找一份技术氛围更好，和薪酬更高的工作，工作强度适当强一些也可以接受。

## 问题列表
- code review 和单元测试

### JVM
- 内存区域 & 内存模型
  - 按照规范：内存区域分为堆、栈、原生方法栈、方法区、PC。
  - 堆区域按不同的垃圾回收器有不同的具体划分，一般基于‘分代收集理论’，将对象分为新生代和老年代两类区域；
  - 方法区据说呗实现为‘永久代’，在 jdk6 时有逐步去除的计划，jdk 8 时完全去除。
  - 内存模型是一种模拟 CPU 缓存结构的模型，分为主存和本地内存

- 类加载
  1. 时机：new、静态字段getset、调用静态方法、反射调用、先初始化父类、初始化 main 类、动态语言？、default 接口
  2. 过程：加载、验证、准备、解析、初始化
  3. 加载：1. 通过类全名来获取二进制字节流；2. 将字节流转换为二进制字节流；3. 生成 Class 对象
    - 二进制字节流没有规定具体来源，所以可以从 zip 读取、网络读取、运行时计算生成、数据库读取、加密文件读取等。

  4. 验证：在 jdk7 规范中有详细描述，验证字节文件符合虚拟机规范
  5. 准备：初始化 static 字段
  6. 解析：符号引用替换为直接引用；比如A 依赖了 com.b.B ，会先让 A 的类加载器去加载 B，并检查访问权限
  7. 初始化：就是执行静态代码块，和 static 赋值（`<clinit>`方法）。这方法是同步加锁的。
    同一个类加载器下，一个类型只会初始化一次。、
  8. 双亲委派模型：规范中只描述启动类加载器（C++的）和其他加载器两种，但 java 语言层面上可能会有三层。
    - 启动类加载器 (Bootstrap Class Loader) 加载 $JAVA_HOME/lib 下面指定名称的内容。
    - 扩展类加载器 (Extension Class Loader) 加载 $JAVA_HOME/lib/ext 中的类，涉及用于扩展 java 本身功能。
    - 应用程序类加载器，加载 classpath 上的类，如果没有自定义类加载器，这个是默认的。
    - 双亲委派模型：除了启动类加载器之外，所有的类加载器必须有父类加载器。
      若一个类加载器收到类加载请求，只有父类加载器无法加载类型时，才会自己去加载。

- ConcurrencyHashMap
  - 基于 CAS 机制实现
  - jdk 7 基于分段锁实现

- synchronized

- volatile

- LatchDown

- 反射 API


### MySQL
- 事务的实现
- 索引数据结构

### Spring
- 谈谈了解的 Spring、基本工作流程
- Spring Boot 自动配置


### 分布式事务

### 中间件
- Redis
- HDFS

### 算法
- DP
- 贪心
- 快速排序

### MyBatis
- 设计目标
- 调用流程 & 基本原理

### 项目经验
-

### 其他
- 红黑树
