# 1. java多线程

OOM = out of memory

[java多线程](https://xiaolincoding.com/interview/juc.html#%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%88%9B%E5%BB%BA%E6%96%B9%E5%BC%8F%E6%9C%89%E5%93%AA%E4%BA%9B)

## 1.1. 创建多线程的方法

### 1.1.1. 继承Thread，重写run方法

1. 编程简单，访问当前线程无需Thread.currentThread，直接this访问。。。
2. 不能继承其他父类啊

### 1.1.2. 实现runable接口

1. cpu代码和数据分开？？？

### 1.1.3. 实现callable接口

callable是juc包的，重写call方法，并将其作为对象传入FutureTask中

再将callable作为参数传入到Thread对象中，可以通过futureTask.get获取线程执行的结果

### 1.1.4. 线程池

[线程池](https://javaguide.cn/java/concurrent/java-thread-pool-summary.html#%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%8F%82%E6%95%B0%E5%88%86%E6%9E%90)

```java
/**
 * 用给定的初始参数创建一个新的ThreadPoolExecutor。
 */
public ThreadPoolExecutor(int corePoolSize,//线程池的核心线程数量
                            int maximumPoolSize,//线程池的最大线程数
                            long keepAliveTime,//当线程数大于核心线程数时，多余的空闲线程存活的最长时间
                            TimeUnit unit,//时间单位
                            BlockingQueue<Runnable> workQueue,//任务队列，用来储存等待执行任务的队列
                            ThreadFactory threadFactory,//线程工厂，用来创建线程，一般默认即可
                            RejectedExecutionHandler handler//拒绝策略，当提交的任务过多而不能及时处理时，我们可以定制策略来处理任务
                            ) {
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

三大核心参数：corePoolSize、maximumPoolSize、workQueue

1. 核心线程数
2. 最大线程数
3. 多余线程存活最长时间
4. 3的时间单位
5. 任务队列
6. 创建线程的线程工厂
7. 拒绝策略是什么？

比较常见的线程池创建方法：

1. FixedThreadPool：固定线程数量的线程池。该线程池中的线程数量始终不变。当有一个新的任务提交时，线程池中若有空闲线程，则立即执行。若没有，则新的任务会被暂存在一个任务队列中，待有线程空闲时，便处理在任务队列中的任务。
2. SingleThreadExecutor： 只有一个线程的线程池。若多余一个任务被提交到该线程池，任务会被保存在一个任务队列中，待线程空闲，按先入先出的顺序执行队列中的任务。
3. CachedThreadPool： 可根据实际情况调整线程数量的线程池。线程池的线程数量不确定，但若有空闲线程可以复用，则会优先使用可复用的线程。若所有线程均在工作，又有新的任务提交，则会创建新的线程处理任务。所有线程在当前任务执行完毕后，将返回线程池进行复用。
4. ScheduledThreadPool：给定的延迟后运行任务或者定期执行任务的线程池。

1 和 2 是一类的，2 是 1 的特殊情况版，队列上限是Integer.MAX_VALUE

3 的 固定线程数是0，最大线程数上限为 Integer.MAX_VALUE，容易造成线程的大量堆积

4 是啥？不懂，这个定时是怎么算的？没看源码？

## 1.2. execute() vs submit()

先说结果：简单来说，execute直接运行，提交runable任务，没有返回，但是会响应异常；submit只是提交了任务，不会主动抛出异常，但是可以通过get方法获取结果+异常，处理更加灵活。

execute() 和 submit()是两种提交任务到线程池的方法，有一些区别：

返回值：

execute() 方法用于提交不需要返回值的任务。通常用于执行 Runnable 任务，无法判断任务是否被线程池成功执行。

submit() 方法用于提交需要返回值的任务。可以提交 Runnable 或 Callable 任务。submit() 方法返回一个 Future 对象，通过这个 Future 对象可以判断任务是否执行成功，并获取任务的返回值（get()方法会阻塞当前线程直到任务完成， get（long timeout，TimeUnit unit）多了一个超时时间，如果在 timeout 时间内任务还没有执行完，就会抛出 java.util.concurrent.TimeoutException）。

异常处理：在使用 submit() 方法时，可以通过 Future 对象处理任务执行过程中抛出的异常；而在使用 execute() 方法时，异常处理需要通过自定义的 ThreadFactory （在线程工厂创建线程的时候设置UncaughtExceptionHandler对象来 处理异常）或 ThreadPoolExecutor 的 afterExecute() 方法来处理。

## 1.3. 多线程处理异常

可以在创建多线程的时候，重写afterExecute方法，让submit和execute都抛出异常，但是我觉得没必要啊，各有各的特点，没必要强行抛出

## 1.4. 线程池的最佳实现

根据不同的情况自定义实现线程池，不要用Executors提供的那四个

小心父与子共用一个线程池导致的死锁掐架问题

线程池取名字，利用 guava 的 ThreadFactoryBuilder，ThreadFactory，这个工厂可以指定名字，不过线程池的名字确实没办法，毕竟存在线程共用的情况，名字没办法真真实实的一对一

线程池参数 cpu 密集任务 n+1，io密集（网络读取、文件读取） 2n 

## 1.5. 记得关闭线程池

尤其是任务结束的时候清空ThreadLocal，因为线程池会复用，这玩意不清除下一次会接着用的，不过看情况吧，万一就是希望保留ThreadLocal参数呢

## AQS

什么是AQS？？？是一个用于构建锁、同步器、协作工具类的工具类。

核心思想是，如果请求的共享资源空闲，则将当前请求资源的线程设置为有效工作线程，并将工作资源设置为锁定状态。如果请求的共享资源被占用，那就需要一套线程阻塞+唤醒分配机制。CLH队列的变体实现这个机制，将暂时获取不到的锁的线程加入到队列中。

同步线程原子性管理、阻塞线程+唤醒线程、队列管理。

## CAS

ABA问题、循环时间长开销大、只对当个共享变量有效