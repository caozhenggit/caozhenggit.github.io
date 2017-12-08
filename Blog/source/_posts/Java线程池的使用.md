---
title: Java线程池的使用
date: 2017-10-27 11:30:18
categories: "Java"
tags: "线程"
---

## new Thread与线程池 ##
**new Thread的弊端**

-  每次new Thread新建对象性能差。
-  线程缺乏统一管理，可能无限制新建线程，相互之间竞争，及可能占用过多系统资源导致死机或oom。
-  缺乏更多功能，如定时执行、定期执行、线程中断。

**相比new Thread，Java提供的四种线程池的好处在于：**

-  重用存在的线程，减少对象创建、消亡的开销，性能佳。
-  可有效控制最大并发线程数，提高系统资源的使用率，同时避免过多资源竞争，避免堵塞。
-  提供定时执行、定期执行、单线程、并发数控制等功能。

## 四种线程池 ##

### newCachedThreadPool ###

创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。

``` bash
ExecutorService cacheThreadPool = Executors.newCachedThreadPool();
 cacheThreadPool.execute(runnable);

```


### newFixedThreadPool  ###

创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。

``` bash
ExecutorService fixedThreadPool = Executors.newFixedThreadPool(4);       
fixedThreadPool.execute(runnable);

```

### newScheduledThreadPool  ###

创建一个定长线程池，支持定时及周期性任务执行。

``` bash
ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(4);
 scheduledThreadPool.schedule(runnable3, 2000, TimeUnit.MILLISECONDS);

```

### newSingleThreadExecutor  ###

创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。

``` bash
ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();
singleThreadExecutor.execute(runnable);

```