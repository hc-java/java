IDEA激活码：<http://idea.medeming.com/jet/>

# Spring基础

## IoC

中文名：控制反转

目的：降低耦合度

简介：常用**依赖注入**的方式（**基于注解注入方式**、set注入方式、构造器注入方式、静态工厂注入方式），创建对象的容器，降低代码间的耦合度。

### 依赖注入

​		有IoC容器动态地将某个对象所需要的外部资源注入到组件（Controller、Service）之中。

​		就是IoC容器会把当前对象所需要的外部资源动态的注入给我们。

例如：在一个Controller层中，想要调用service层中的某一个方法，在没有框架的情况下，是先new一个service层的对象，然后在 对象.方法 。然而在使用SSM框架的时候，可以用@Autowired先声明一个私有的service层对象，然后 对象.方向  调用。

​		总结：用**基于注解注入**方式调用函数的某个方法明显少了一个new对象的操作。



## AOP

中文名：面向切面编程

目的：把公共的功能模块提取出来，在需要的时候再放进去。（不是函数调用）

应用：日志记录、异常处理、权限控制、性能统计、签名验签、参数校验、事务控制等。



例如：1.先引用AOP的依赖

~~~xml
  		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
~~~



​	   2.在一个类上加上Aspect注解

​	   3.在类下面 加Before、After这两个注解

~~~java
@Component
@Aspect
public class UserVerifyAspect {
    @Before("execution(public * com.hc.znsc.controller.*.*(..))")
    public void verify() {
        System.out.println("用户验证");
    }

    @After("execution(public * com.hc.znsc.controller.*.*(..))")
    public void verification() {
        System.out.println("验证通过");
    }
}
~~~



网上的图解：

aspect   方面

execution 执行



![1578801229137](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1578801229137.png)



## 动态代理

AOP框架不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。

## Spring Bean生命周期

实例化，初始init，接收请求service，销毁destroy

# SpringBoot基础

## 自动配置



- springboot启动时加载@SpringBootApplication主配置类
- @SpringBootApplication开启@EnableAutoConfiguration自动配置类
- @EnableAutoConfiguration利用@Import里面的AutoConfigurationImportSelector给容器中导入一些组件
- 查看public String[] selectImports(AnnotationMetadata annotationMetadata)方法的内容。
- 通过protected List<String> getCandidateConfigurations(AnnotationMetadata metadata,      AnnotationAttributes attributes)获取候选的配置，
- SpringFactoriesLoader.loadFactoryNames这个是扫描所有jar包类路径下"META-INF/spring.factories";然后把扫描到的这些文件包装成Properties对象。
- 从properties中获取到EnableAutoConfiguration.class类名对应的值，然后把他们添加在容器中。
- 整个过程就是将jar包类路径下"META-INF/spring.factories"里面配置的所有EnableAutoConfiguration的值加入到容器中。



- 点击@SpringBootApplication

  ![1578214606495](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1578214606495.png)

- 点击@EnableAutoConfiguration

  ![1578214627101](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1578214627101.png)

- 点击@Import({AutoConfigurationImportSelector.class})中的AutoConfigurationImportSelector

  ![1578214647979](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1578214647979.png)

- 点击List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes); 中的getCandidateConfigurations  （候选配置）

  ![1578214694606](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1578214694606.png)

- 点击SpringFactoriesLoader.loadFactoryNames  （工厂装载）

  ![1578214714338](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1578214714338.png)

- SpringFactoriesLoader.loadFactoryNames的作用是扫描所有jar包类路径下"META-INF/spring.factories"，然后将扫描到的文件包装成Properties对象。

  ![1578214744378](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1578214744378.png)

  ![1578214787128](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1578214787128.png)

  ![1578215223973](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1578215223973.png)

- XxxxAutoConfiguration类的含义是：自动配置类，目的是给容器中添加组件

- XxxxProperties类的含义是：封装配置文件中相关属性





## 场景启动器

Spring boot将所有的功能场景，抽取出来
做成一个个独立的starters启动器

只有在项目里面，引入了starters
这些场景的所有依赖，都会导出到工程中
需要什么样的功能，引入什么场景启动器



# mybatis、Hibernate的区别

**Mybatis**：小巧、方便、高效、简单、直接、半自动化

**Hibernate**：强大、方便、高效、复杂、间接、全自动化



**Mybatis**：

是一个半自动化的持久层框架。

sql需要手工完成

手动编写sql，可以避免不需要的查询，提高系统性能；

性能要求高、响应快、灵活

 **Hibernate**：

是一个全自动的、完全面向对象的持久层框架。

sql语句已经被封装，直接可以使用

自动生成sql,有些语句较为繁琐，会多消耗一些性能；





# 过滤器、拦截器

拦截器功能更强大

拦截器（Interceptor）是基于Java的反射机制，而过滤器（Filter）是基于函数回调。

（1）**过滤器(Filter)**：它依赖于servlet容器。在实现上，基于函数回调，它可以对几乎所有请求进行过滤，但是缺点是一个过滤器实例只能在容器初始化时调用一次。使用过滤器的目的，是用来做一些过滤操作，获取我们想要获取的数据，比如：在Javaweb中，对传入的request、response提前过滤掉一些信息，或者提前设置一些参数，然后再传入servlet或者Controller进行业务逻辑操作。通常用的场景是：在过滤器中修改字符编码（CharacterEncodingFilter）、在过滤器中修改HttpServletRequest的一些参数（XSSFilter(自定义过滤器)），如：过滤低俗文字、危险字符等。

基于函数回调，依赖servlet容器。

目的：过滤一些操作（过滤低俗文字、危险字符等）

优点：可以对几乎所有请求进行过滤

缺点：一个过滤器实例只能在容器初始化调用一次



（2）**拦截器（Interceptor）**：它依赖于web框架，在SpringMVC中就是依赖于SpringMVC框架。在实现上,基于Java的反射机制，属于面向切面编程（AOP）的一种运用，就是在service或者一个方法前，调用一个方法，或者在方法后，调用一个方法，比如动态代理就是拦截器的简单实现，在调用方法前打印出字符串（或者做其它业务逻辑的操作），也可以在调用方法后打印出字符串，甚至在抛出异常的时候做业务逻辑的操作。由于拦截器是基于web框架的调用，因此可以使用Spring的依赖注入（DI）进行一些业务操作，同时一个拦截器实例在一个controller生命周期之内可以多次调用。但是缺点是只能对controller请求进行拦截，对其他的一些比如直接访问静态资源的请求则没办法进行拦截处理。

基于java的反射机制，属于AOP的一种运用。依赖web框架。

优点：同一个拦截器实例可以在一个Controller生命周期之内多次调用

缺点：只能在Controller请求进行拦截，其它的请求无法拦截

