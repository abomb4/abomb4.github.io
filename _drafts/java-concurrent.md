---
layout: post
title:  "Java concurrent 包的学习"
categories: Java
date: +0800 2020-06-16 21:38:26
tag: Java
---

* content
{:toc}

# 目标
--------------
记录整个 `java.util.concurrent` 包的学习过程。

## ReentrantLock
是一个可重入锁，支持公平和非公平锁模式。

与 `synchronized` 的相同点包括：
- 都可重入

不同点：
- 支持公平模式
- 使用方式更加灵活

其实现原理为基于 `AbstractQueuedSynchronizer` 实现的同步机制。

## AQS
一般指 `AbstractQueuedSynchronizer`，它是一个框架，
用于实现基于 FIFO 队列的阻塞锁和相关的同步器入信号量、事件等。

基本上，这个框架提供的是基于原子 `int` 操作的各类同步机制。

典型应用包括 `CountDownLatch`, `Semaphore`, `ReentrantLock`, `ReentrantReadWriteLock`，
以及线程池 `ThreadPoolExecutor` 中的 `Worker`。

实现此框架则会对外开放这些方法：
- `acquire`
- `release`
- `acquireShared`
- `releaseShared`
- `tryAcquireNanos`
- `tryAcquireSharedNanos`

这些方法需要用到框架中直接抛异常的方法，他们是：
- `tryAcquire`
- `tryRelease`
- `tryAcquireShared`
- `tryReleaseShared`

通过实现上面 4 个方法来实现各种同步操作。
实现上面 4 个方法的方法是，基于 `CAS` 机制的 `State` 操作，他们是：
- `getState`
- `setState`
- `compareAndSetState`

通过这些方法来实现各种 try 方法，就可以实现自己想要的同步器了。

以可重入锁的实现来讲，我们可以认为，通过 `acquire(1)` 获取了状态 1 的排斥锁，那么就是取到锁了。
如果没有获取到 1 状态，但排斥线程是自己，那么说明是重入状态，将 `state + 1` 即可实现重入判断。
如果没有获取到 1 状态，排斥线程不是自己，那说明自己没拿到锁，要等。

等待就是同步器将当前线程封装成一个双向链表的 Node 插入链表尾部，以线程安全的方式插入，然后 park。

可重入锁的释放就是将 state - 1，若到了 0，视为已释放。
已释放的话，则会寻找后继节点进行唤醒（unpark）；找后继结点的过程中若遇到已经 cancel 的节点会跳过。

以上分析为 AQS 独占式锁的基本机制。

共享式的
