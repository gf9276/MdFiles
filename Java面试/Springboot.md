# Springboot启动的流程


面试鸭 结合 https://blog.csdn.net/weixin_46047193/article/details/123557570

1. SpringApplication.run
2. 创建一个SpringApplicationRunListeners实例进行监听（规定了SpringBoot的生命周期，在各个生命周期广播相应的事件）
3. 加载SpringBoot配置环境（ConfigurableEnvironment），并将其加入监听对象
4. 获取应用上下文，调用refreshContext刷新Spring上下文（包括自动装配，spring.factories的加载，bean的实例化等核心工作）
5. refreshContext里面顺着往下执行，到了一个OnRefresh方法里，里会调用createWebServer，getWebServer，启动web容器 + 创建web服务器

# SpringApplicationRunListeners

https://www.jianshu.com/p/b86a7c8b3442

https://segmentfault.com/a/1190000040796999（这个写的真的很好）

Spring Boot 启动初始化的过程中可以通过SpringApplicationRunListener接口回调来让用户在启动的各个流程中可以加入自己的逻辑。

EventPublishingRunListener 继承了 SpringBootApplicationRunListener 和 Order 接口，为其他实现该接口的类广播事件

继承SpringBootApplicationRunListener接口就行，然后放到META/INF下的spring.factories里

