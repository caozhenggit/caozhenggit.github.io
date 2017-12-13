---
title: Android开发中定制自己的线程池
date: 2017-8-27 17:05:04
categories: "Android"
tags: "线程"
---

线程池算是Android开发中非常常用的一个东西了，只要涉及到线程的地方，大多数情况下都会涉及到线程池。Android开发中线程池的使用和Java中线程池的使用基本一致。那么今天我想来总结一下Android开发中线程池的使用。

## ThreadPoolExecutor的构造方法 ##

ThreadPoolExecutor的构造方法有四个，如下:

``` bash
    public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime,
                               TimeUnit unit, BlockingQueue<Runnable> workQueue) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
                Executors.defaultThreadFactory(), defaultHandler);
    }

    public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime,
                              TimeUnit unit, BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
                threadFactory, defaultHandler);
    }

    public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime,
                              TimeUnit unit, BlockingQueue<Runnable> workQueue,
                              RejectedExecutionHandler handler) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
                Executors.defaultThreadFactory(), handler);
    }

    public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime,
                              TimeUnit unit,BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory, RejectedExecutionHandler handler) {
        if (corePoolSize < 0 ||
                maximumPoolSize <= 0 ||
                maximumPoolSize < corePoolSize ||
                keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }
```

**构造方法参数说明**

- corePoolSize

	线程池的核心线程数，即线程池中的最小线程数

- maximumPoolSize

	最大线程池大小,当活动线程数达到这个值，后续任务会被阻塞

- keepAliveTime

	线程池中超过corePoolSize数目的非核心线程最大存活时间

- unit

	keepAliveTime 参数的时间单位

- workQueue

	执行前用于保持任务的队列，也就是线程池的缓存队列

- threadFactory

	线程工厂，为线程池提供创建新线程的功能

- RejectedExecutionHandler

	线程池对拒绝任务的处理策略


## 定制自己的线程池 ##

``` bash
public class ThreadPoolFactory {

    /** 核心线程数 */
    private static final int CORE_POOL_SIZE = 5;
    /** 最大线程数 */
    private static final int MAX_POOL_SIZE = 128;
    /** 非核心线程时超过时长 */
    private static final int KEEP_ALIVE_TIME = 2;
    /** 阻塞队列大小 */
    private static final int BLOCK_SIZE = 2;

    private ThreadPoolExecutor mThreadPoolExecutor ;
    
    private ThreadPoolFactory(){
        createThreadPoolProxyFactory();
    }

    /** 创建线程池 */
    private void createThreadPoolProxyFactory(){
        mThreadPoolExecutor  = new ThreadPoolExecutor(CORE_POOL_SIZE, MAX_POOL_SIZE, KEEP_ALIVE_TIME,
                TimeUnit.SECONDS, new ArrayBlockingQueue<Runnable>(BLOCK_SIZE),
                Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy());
        //true: 线程池数量最后销毁到0个
        //false: 超过核心线程数时,而且(超过最大值或者timeout过),就会销毁
        mThreadPoolExecutor .allowCoreThreadTimeOut(true);
    }

    /**
     * 打印线程池状态
     */
    public void logThreadPoolInfo() {
        Log.i("threadPoolFactory", "monitor "
                + " CorePoolSize:" + mThreadPoolExecutor.getCorePoolSize()
                + " PoolSize:" + mThreadPoolExecutor.getPoolSize()
                + " MaximumPoolSize:" + mThreadPoolExecutor.getMaximumPoolSize()
                + " ActiveCount:" + mThreadPoolExecutor.getActiveCount()
                + " TaskCount:" + mThreadPoolExecutor.getTaskCount());
    }
```

