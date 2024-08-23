1. 红黑树和AVL 都是自平衡的二叉搜索树，红黑树没有平衡因子，所以不是严格平衡的，它通过五个特性达到一种近乎平衡的程度。叶子节点都是黑，根节点都是黑，节点不是黑就是红，任意节点到其叶子节点的黑色节点数量一样，从根节点到叶子节点，不存在连续两个红。红黑树因为不需要严格的平衡，所以他旋转的次数会小于avl，整体效率会更高。hashmap就用了红黑树，当链表长度大于8，且数组长度大于64（初始长度16），链表会转红黑树。
2. 为什么hashmap的长度是2的幂次，因为对长度取模难度更低。可以优化成与操作，更快。
3. TreeMap TreeSet
4. 布隆过滤器 超大的二进制数组+多个无偏hash函数组成。一个输入值通过多个无偏hash函数映射再取模后，得到二进制数组的多个索引，如果对应索引的值都为1，那么可能存在，也可能不存在。但是只要，有一个不为1，那就肯定不存在。起到一个过滤作用
5. java有哪些锁？ReetrantLock sychronized
6. IO BIO NIO AIO 同步阻塞IO 同步非阻塞IO 多路复用IO 

BIO 同步阻塞IO

NIO 是一种面向缓存的，使用了通道和选择器，实现的一种IO多路复用模型，他是非阻塞的，通过selector进行select epoll调用查询资源是否就绪，然后再阻塞读取，可以高效的处理大量并发连接。

NIO提供了一种基于缓冲区和通道的I/O处理方式，相较于传统的流式I/O（BIO），NIO支持非阻塞I/O操作，可以更高效地处理大量的并发连接。buffered channel selector（选择select或者epoll实现高效的IO多路复用）

AIO，进一步的，AIO是异步的，因为她是异步读取资源，由内核态直接把资源准备好。


7. select poll epoll。文件描述符
8. java虚拟机内存分布。程序计数器：存储当前线程所执行的字节码的行号计数器。java虚拟机栈：局部变量表，操作数栈、方法出口、动态链接等信息。本地方法栈：服务于Native修饰的方法。堆，对象实例。方法区：元空间，静态变量、常量、类信息。
9. 创建类的方法。new 序列化 clone 反射（Class，Constructor）
10. todo使用redis存储管理gpu资源。
11. 你的项目里使用了哪些设计模式。
12. 你有用到AOP吗？
13. 有用到事务吗？事务的传播。

| 名字         | 存在                                                                           | 不存在                 |
| ------------ | ------------------------------------------------------------------------------ | ---------------------- |
| requires     | 加入                                                                           | 创建新事务             |
| supports     | 加入                                                                           | 就不存在，以非事务运行 |
| mandatory    | 加入                                                                           | 报错                   |
| requires new | 挂起，创建新的                                                                 | 创建新的               |
| not support  | 挂起，非事务运行                                                               | 非事务运行             |
| never        | 报错                                                                           | 非事务运行             |
| nested       | 创建一个新的，嵌套进去，作为子事务，自己的错不会影响外面的，外面的会影响自己的 | 创建一个新的           |


14. 设计模式，用到了哪些设计模式
15. AQS锁的具体过程 一个线程尝试去获取一个资源，如果该资源是空闲的，那么他会把这个线程给置为工作线程，并且把资源锁定。如果资源被锁定了，那么他会将该线程阻塞起来，直到资源释放后唤醒该线程。具体来讲，AQS（抽象队列同步器）通过state和阻塞队列（CLH队列）实现，通过不断CAS重试设置state获取锁。
16. spring bean 的生命周期
17. threadlocal的原理
18. session和cookie的区别 session存于服务端，一般用来存储用户的登录信息等等，且没有最大内存限制。cookie存在于客户端，一般用于存储用户的偏好，或者用户信息，最大上限为4k。
19. 三色标记法
20. java虚拟机内存模型 线程独有 虚拟机栈（局部变量表。。。）、本地方法栈（native修饰的方法，不是用Java实现的）、程序计数器（程序执行机器码的行数）。线程共享的有 堆（字符串常量池（这是个例外，在老年代，其他的在元空间）、Eden、S1、S2、老年代）、方法区（元空间：类信息、常量、静态变量）
21. 有哪些线程安全的集合 ConcurrentHashMap、WriteOnCopyArrayList
22. hashmap为什么是线程不安全的 没有任何应对多线程的措施，当 多线程并发添加元素时，如果发生hash碰撞，容易导致元素丢失。1.7以前因为头插法，还会导致循环问题。
23. ip可以传播信息，为什么还要tcp？（tcp保证了连接的有效性，超时重传。。。）
24. sql里的having
25. 对象创建的过程 类加载查询 分配内存（指针碰撞 内存队列，cas+错误重试、eden分配一个大的内存） 初始值0 对象头（包括元数据、GC、哈希码） 执行构造函数
26. Object的11个方法 hashcode tostring clone wait wait wait notify notifyall finalize equals getClass!
27. 为什么不用UUID做MYSQL主键 --> 不是有序的，会导致页分裂，树裂变问题。为什么不用雪花算法 --> 有序递增 唯一，但是有时钟回拨问题，要小心。分布式为什么不用自增主键，服务器同步消耗资源。 [参考连接](https://www.bilibili.com/video/BV1tx4y1s7Sd/?buvid=XY1FD0E11608521D4F0EDD39DFADF4E67F559&from_spmid=tm.recommend.0.0&is_story_h5=false&mid=MEfEQV3r8A8D9wFLkkEQYw%3D%3D&p=1&plat_id=116&share_from=ugc&share_medium=android&share_plat=android&share_session_id=355bbaf3-2e6a-4118-aec9-4366747feb94&share_source=WEIXIN&share_tag=s_i&spmid=united.player-video-detail.0.0&timestamp=1723629119&unique_k=XF9X1wt&up_id=1895753405)
28. 进程间通信的方式 管道 信号量 共享内存区 消息队列 socket 信号
29. 部署socket 服务端 打开网络库 创建socket 绑定端口和Ip 监听 接受连接 socket bind listen accept read/write
30. 客户端呢 打开网络库，创建socket 连接服务器
31. RESTFUL风格，一种软件架构风格，简单来说，URL里只有名词来定位资源，使用HTTP协议的动词（DELETE GET PUT PATCH）来实现资源的增删改查。[参考链接](https://blog.csdn.net/SeniorShen/article/details/111591122)
32. 幂等性是一个数学和计算机科学中的基本概念，主要应用于抽象代数领域。在编程中，一个幂等操作的特点是其任意多次执行所产生的影响均与一次执行的影响相同。这意味着无论一个操作被执行多少次，其结果始终保持不变
33. redis 如何创建分布式锁？
34. redisson用什么依赖？
35. 如何使用redis构造消息队列？
36. get和post的区别 get把参数放在请求行里（所以有长度限制），post把参数放在请求体里。get只做查询，post用于修改。post不是幂等的，get是。get可以被缓存，post不行
37. 并行GC和并发GC
38. CMS [CMS并发收集器](https://cloud.tencent.com/developer/article/2397939)
39. G1收集器 1024个region，每个region最大32MB。会选垃圾多的先进行回收，但是呢，他还是要标记整理，还是浪费时间 [写的很好](https://segmentfault.com/a/1190000039411521)  [理解G1垃圾收集器](https://blog.csdn.net/wanghang96/article/details/109731479)
40. G1收集器 完全年轻代回收 和普通的一样，从一个region复制到另一个region，就是复制算法。而且是STW的，上面两个链接都有说到这件事情。[这里也有](https://cloud.tencent.com/developer/article/1824886)
41. G1收集器 混合回收 回收年轻代+老年代，初次标记（STW）、并发标记、根区域扫描Rset、最终标记（STW）、回收（STW） [kan](https://www.jianshu.com/p/0b978e57d430) 为什么要跟区域扫描？为什么其他gc没有这种问题？我认为主要原因是，G1是分region清理的，而不是整块堆一起清理的[看这个](https://cloud.tencent.com/developer/article/1824880)
42. 哪些可以做为root?
43. 什么是增量回收? 不是一次把垃圾都回收完，而是递进的回收。
43. 什么是三色标记法 没访问到白色 访问到了灰色 访问到直接子节点了黑色 错判（写屏障+快照 写屏障+增量更新） 漏判 [参考链接](https://blog.csdn.net/star1210644725/article/details/115712443)
44. 锁升级 偏向锁 轻锁（CAS） 重锁（互斥）
45. 有哪些常见的starter
46. reddsion如何实现阻塞队列 reddsion实现了blockingQueue，实现了juc包里的blockingqueue接口，本质还是List数据结构
47. 有哪些线程安全的集合？concurrentHashMap （Node CAS + sychronized）CopyOnWriteArrayList blockingQueue
48. netty 封装了NIO，超牛逼的网络编程框架 [netty](https://cloud.tencent.com/developer/article/1748761)
49. Err和exception，err是没法解决的异常，java虚拟机出错了，出现这个一般会直接停止程序。exception是可以捕获的。
50. 受检查异常、非检查异常（runtimeException及其子类，例如空指针异常或者数组越界之类的，不需要用户throws或者try catch的）
51. 有几个拒绝策略？abort（抛出异常） caller discard discardold
52. 自动装配流程？
53. spring boot项目的创建流程？
54. 什么情况下会oom？
55. 用了什么设计模式？你知道哪些设计模式？单例、观察者
56. 最小生成树是什么？
57. java多线程是用户线程吗？（不是的）
58. 线程数多少合适？
59. netty的应用（Spring Cloud gateway和redisson）
60. JWT的消息存在哪里？redis
61. JWT分为几个部分？3个64位
62. header里的authorization （token放的位置）
63. 哪些用了单例模式？GPU资源管理（因为GPU资源在服务器上数量肯定是固定的嘛，所以创建了一个单例的类去管理他），连接池、单例的spring bean（默认的）。适配器模式：数据集的格式很多，gdsx、h5、csv、甚至他们还把数据附着在请求体里，因为一开始定下的是h5格式的，所以一开始也只写了h5格式的接口，后面为了解决数据集格式不同的问题，中间加了一个转换层，对不同格式的数据集统一转换成h5格式，这样就能兼容以前写的接口了。建造者模式：用户通过自定义的具体配置（模型参数、训练参数），指挥创建对应的任务实例（包括工作空间）。模板方法：没有用在java里，写python训练代码的时候用的，像我们做深度学习训练或者预测的时候，整体的流程是固定的，创建模型、加载数据、加载到GPU里，创建优化器、创建学习率优化器，然后开始训练/预测，然后做训练，训练过程中还要进程日志记录。不同任务类别，细节处可能不一样，但是大致的流程是差不多的，我针对测井任务写了一个整体流程模板给我们实验室用，他们训练的时候自己调用钩子函数，或者重写stepRunner或者epochRunner就可以实现自定义训练+观测。
64. 观察者模式 spring 事务，java io，nio也用了观察者吧？
65. 如何创建线程池的starter？
66. 有哪些springboot starter? mybatis redisson redisdata springbootcloud mybatisplus web parent
67. 遇到过哪些问题？openfeign调用丢失头、读取输出死锁问题（如何检测死锁，https://blog.csdn.net/qq_34680444/article/details/109642653）、如何访问指定的IP、数据集标签返回的特别慢
68. 内存没有释放掉的原因（生命周期不合理、单例模式引用、静态变量引用、常量引用）
69. 触发 full gc 的前提条件 老年代区域不够、元空间区域不够
70. spring bean的作用域 单例 singleton prototype request session application websocket
71. spring bean的生命周期？实例化 依赖注入 初始化（myInit） 使用bean 销毁bean
72. 多线程处理数据集。。。
73. 建造者模式和工厂模式的区别
74. 对修改关闭，对扩展开放
75. 对象的创建过程 类加载检查 --> 分配内存（指针碰撞，空闲列表，线程本地分配缓存TLAB + CAS） --> 初始化零值 --> 设置对象头（哈希码，GC分代年龄） --> 执行init方法
76. 加载 验证 准备 解析 初始化 使用 卸载
77. jconsole，慢sql问题
78. 类加载器，加载Java类字节码到JVM中，在内存中生成类的Class对象（用到再加载，就跟那啥一样的嘛，类加载检查）
79. 双亲委派，子加载器再接受到类加载请求时，会把这个请求传递给父加载器，如果父加载器无法加载，他才会自己去尝试加载。确保类不会被重复加载。核心API不被篡改
80. Spring bean的创建过程，生命周期，实例化，依赖注入，初始化，使用，销毁
81. Spring boot 自动装配注解，什么是自动装配？
82. jps
83. 看一下mysql和redis+集群
84. 看一下Spring springboot spring cloud
85. @Component 和 @Bean 的区别是什么？@Component 注解作用于类，而@Bean注解作用于方法。@Component通常是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中（我们可以使用 @ComponentScan 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。@Bean 注解通常是我们在标有该注解的方法中定义产生这个 bean,@Bean告诉了 Spring 这是某个类的实例，当我需要用它的时候还给我。@Bean 注解比 @Component 注解的自定义性更强，而且很多地方我们只能通过 @Bean 注解来注册 bean。比如当我们引用第三方库中的类需要装配到 Spring容器时，则只能通过 @Bean来实现。
86. @Autowired （Qualifier）和 @Resource 的区别是什么？
87. spring循环依赖 三级缓存