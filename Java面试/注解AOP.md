# 注解如何创建

注解继承了anotation的接口，靠动态代理实现。

就是用动态代理获取注解的值。field.getAnnotation，其实也没啥。。。

## 接口

@Target({ ElementType.PARAMETER, ElementType.METHOD }) // 可以放在哪些头上
@Retention(RetentionPolicy.RUNTIME) // 注解的生命周期 RUNTIME注解不仅被保存到class文件中，jvm加载class文件之后，仍然存在
@Documented

元注解

retention 

runtime 运行过程中可以用反射动态的获取对应的值

@Override就是source，他只是做检查工作


@Class，编译有，运行就没有了。比如Lombok 

感觉除了runtime都要靠AbstractProcessor 注解处理器来实现啊。。。


## 切面

@Aspect
@Component

定义切入点 --> Log的全限定名，AfterReturning，AfterThrowing，这样就实现了


