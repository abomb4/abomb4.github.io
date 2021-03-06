---
layout: post
title:  "对业务架构的一点思考"
categories: 架构
date: +0800 2018-01-15 17:18:36
tag: 架构
---

* content
{:toc}

# 当前所遇到的痛点/难点
---------------

## 实例1
------------
当前我们小组需要新开一个项目，但由于时间不够，再加上本来公司其他项目组已经有一个类似的项目，
所以就把他们的代码直接拿来用了。

本来拿来用的问题不大，最多学习成本多一点，在上面进行改造的话问题也不会非常难，
但现在有个情况，就是项目组决定使用现有代码之前，已经花很多时间做了需求分析与总体设计，
包括技术选型、开发框架搭建、数据库设计 都已经做好了。

此时系统中就存在了两套数据表，他们彼此有交集，但又有很大区别。

这时面临一个选择：是放弃我们的表与框架，完全用别人的；，还是做兼容开发，逐步迁移；还是整个不要，重新开发，如果原代码有相应功能则复制代码？

受限于**时间要求**，再加上原代码学习起来有点困难，又不想放弃原代码已有的功能，
又不想放弃已经做好的需求与设计，于是我们采用了最不好的办法：兼容开发。

这样就导致，当前版本的系统有两种不同的代码结构与数据库表结构共存。
新旧表很多功能是重合的，但我们只能以其中一个为准，这时我们选用我们新建的库为准，毕竟更熟悉。

不过有很多功能性的业务，都是依赖原数据表的，但现在数据库用的是新表，所以需要修改一下表名和字段。
本来想“我们改改原来的DAO不就完事了吗”，但现实完全不是这样的。

经搜索，在所有Mybatis SQLMap中，一共有70+个文件中出现了那个要修改的表的名字……
各种Join 子查询 出现在其中。

这时就发现一个毛病：不同功能的表结构耦合太强。

当然解耦数据表是很难的。

再看向Service与DAO层也有类似的问题：一个Service，会引入其他功能模块的DAO。

原代码结构方面的坑点有：
    1. 有将近170个DAO，一列一大排，在同一个包里
    2. 这170个DAO 也对应170个Service，也在同一个包里，有些有业务逻辑，有些则是简单的查询方法
    3. 当然也对应几乎同等数量的Action，同一个包
    4. 有很多表关联SQL

