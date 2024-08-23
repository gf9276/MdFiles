# redis 如何创建分布式锁？

redisson都封装好了，他会判断

设置锁的时候，使用hash类型，key 锁的名字 field lockName? 包括线程id，value是重入次数。

释放锁的时候，会判断线程id对的上与否

# redisson用什么依赖？

jedis redisson-spring-boot-starter


# 如何使用redis构造消息队列？

用redisson, redisson-spring-boot-starter 无缝替换 spring-boot-starter-data-redis

单例模式，获取redisclient，里面有一个BlockingQueue，实现了juc包里的blockingqueue接口，