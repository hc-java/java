学习网址

<https://www.bilibili.com/video/av38657363?from=search&seid=12162567267604664608>

谷粒学院

## springboot 创建一个项目

### 1.创建一个maven工程

### 2.添加pom.xml依赖

### 3.编写入口

## springboot快速创建一个项目

参考视频：<https://www.bilibili.com/video/av58785586?p=9>

### 1.创建一个项目

选择 spring Intializr

![1575532453232](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575532453232.png)

一直点下一步，然后选择Spring Web

![1575532562016](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575532562016.png)

最后finish完成。

如下图所选中的四个文件可以删除。

![1575638205536](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575638205536.png)

### 2.创建一个controller

代码如下图所示。

![1575532919256](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575532919256.png)

### 3.运行项目

选择application文件,如下图所示,点击Run '...' 

![1575533003889](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575533003889.png)

### 4.访问

![1575533151914](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575533151914.png)

### 5.打包

先在pom.xml加插件

~~~xml
			<plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
~~~



点击package

![1575534337218](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575534337218.png)

打完包后，将这个jar包，复制到桌面(如果用spring initializr创建，可以在本地项目中找到target目录)。

![1575534414531](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575534414531.png)

windows从C盘到E盘：

E：

cd 路径

在控制台中输入 java -jar 包名，然后在去页面访问。

![1575534578710](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575534578710.png)



点击这个clean，可以清除target目录

![1585896127995](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1585896127995.png)





Plugins显示红色：修改User ssettings file和Local repository

![1575538600191](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575538600191.png)

## 项目代码分析

Spring Boot 将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来。要用什么概念就导入什么场景的启动器

### 1.父项目

~~~xml
SpringBoot的版本仲裁中心：管理springboot的依赖版本， 以后导入的依赖默认不需要写版本（没有在dependencies里面管理的依赖依旧需要声明版本号）。
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
~~~

### 2.依赖

~~~xml
spring-boot-starter:spring-boot场景启动器；帮我们导入了Web模块正常运行所依赖的组件。
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
~~~

### 3.主程序类，主入口类



~~~java
package com.hc;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringbootQuickApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootQuickApplication.class, args);
    }

}
~~~

**@SpringBootApplication** :(组合注解，包含很多注解) Spring Boot应用标注在某个类上就说明这个类是SpringBoot的主配置类，SpringBoot就会运行这个类的main方法来启动SpringBoot应用；

### 4.Controller

![1575549781890](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575549781890.png)

## resources文件夹中的目录结构

![1575550080727](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575550080727.png)

### 1.static

保存所有的静态资源；css;js;images

### 2.templates

保存所有的模板页面；（SpringBoot默认jar包使用嵌入式的Tomcat，默认不支持JSP页面）；可以使用模板引擎（freemarker、thymeleaf）;

### 3.application.properties

Spring Boot应用的配置文件，可以修改一些默认配置；比如在application.properties里面写 server.port=8081,可以修改默认的8080端口

## SpringBoot配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的。

### 1.application.properties

配置例子：![1575551541652](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575551541652.png)

### 2.application.yml

YAML（YAML Ain't Markup Language）：以数据为中心，比json、xml等更适合做配置文件

YAML A Markup Language:是一个标记语言

YAML isn't  Markup Language:不是一个标记语言

配置例子：![1575551442266](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575551442266.png)

#### 2.1YAML语法

基本语法：k:(空格)v:(空格)         表示一对键值对（空格必须有）；

以**空格**的缩进来进行控制层级关系；只要是左对齐的一列数据，都是同一个层级的

~~~yaml
server:
	port: 8081
	path: /hello
~~~

#### 2.2值的写法

**字面量：普通的值（数字，字符串，布尔）**

​	k: v :  字面直接来写；

​	字符串默认不用加上单引号或者双引号；

​	""：双引号；不会转义字符串里面的特殊字符

​	''：单引号；会转义特殊字符；

**对象、Map(属性和值)（键值对）**

​	k: v :  在下一行来写对象的属性和值的关系；注意缩进；

​	对象还是k: v: 的方式

~~~yaml
friends:
	lastName: zhangsan
	age: 20
~~~

   行内写法：

~~~yam
    friends: {lastName: zhangsan,age: 20}
~~~

**数组(List、Set)**

用-值表示数组中的一个元素

~~~yaml
pets:
	- cat
	- dog
	- pig
~~~

行内写法

~~~yaml
pets: [cat,dog,pig]
~~~

## 配置文件注入

### 1.配置文件

~~~yaml
#application.yml
person:
  lastName: hello
  age: 20
  boss: false
  brith: 2019/12/12
  maps: {k1: v1,k2: v2}
  lists:
    - lisi
    - wangwu
  dog:
    name: 小狗
    age: 12
~~~

~~~yaml

#application.properties
#IDEA properties默认配置文件 utf-8
person.last-name=张三
person.age=20
person.brith=2019/12/12
person.boss=false
person.maps.k1=v1
person.maps.k2=v2
person.lists=a,b,c
person.dog.name=狗
person.dog.age=2
~~~

application.properties配置如果使用中文，会**乱码**。解决办法如下图：选择UTF-8编码，选中右边的框

![1575557508138](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575557508138.png)

### 2.javaBean

~~~java
/*
* 将配置文件中配置的每一个属性的值，映射到这个组件中
*@ConfigurationProperties:告诉SpringBoot将本类中的所有属性和application.yml配置文件中相关的配置进行绑定
* prefix = "person" :在application.yml配置文件中与属性名为person进行映射
*
* @Component :将下面的代码加到容器中。只有在容器中才能提供@ConfigurationProperties功能；
*@ConfigurationProperties(prefix = "person")默认从全局配置中获取
* */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private  boolean boss;
    private Date brith;

    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
~~~

### 3.导入配置文件

将1.配置文件和2.javaBean连接起来

~~~xml
 <!--导入配置文件处理器，配置文件进行绑定就会有提示-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
~~~

### 4.运行

点击Run '...'

![1575557253229](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575557253229.png)

!![1575558033091](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575558033091.png)1575557221239](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575557221239.png)

![1575557708970](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575557708970.png)    



## @Value 和 @ConfigurationProperties获取值得比较

配置文件yml、properties他们都能获取到值；如果只要一个值，使用@Value;如果要多个值，使用@ConfigurationProperties

![1575558970294](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575558970294.png)

## @PropertySource & @ImportResource

**@PropertySource**

在resources文件夹里面新建一个person.properties,然后把application.properties里面的代码，放到person.properties里面

~~~java
/*
* 将配置文件中配置的每一个属性的值，映射到这个组件中
*@ConfigurationProperties:告诉SpringBoot将本类中的所有属性和application.yml配置文件中相关的配置进行绑定
* prefix = "person" :在application.yml配置文件中与属性名为person进行映射
*
* @Component :将下面的类加到容器中。只有在容器中才能提供@ConfigurationProperties功能；
*
* @PropertySource(value = {"classpath:person.properties"}) ：加载指定的配置文件
* */
@PropertySource(value = {"classpath:person.properties"})
@Component
//@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private  boolean boss;
    private Date brith;

    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
~~~

**@ImportResource**：导入Spring的配置文件，让配置文件里面的内容生效（有多少个bean，注解里面就要写多少个.xml）；

![1575560697284](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575560697284.png)

## SpringBoot推荐给容器中添加组件的方式

1.配置类 ======Spring配置文件

2.使用@Bean给容器中添加组件

新建一个config包，在包里有一个MyAppConfig.java类，代码如下：

~~~java
/*
* @Configuration 指明当前类是一个配置类；就是替代之前的Spring配置文件
* */
@Configuration
public class MyAppConfig {

    //将方法的返回值添加到容器中；容器中这个组件默认的id就是这个方法名
    @Bean
    public HelloService helloService(){
        System.out.println("配置类@Bean给容器中添加了组件");
        return new HelloService();
    }

}


3.test包下的测试类
    @Autowired
    ApplicationContext ioc;
    @Test
    public  void testHelloService(){

        boolean b=ioc.containsBean("helloService");//helloService对应@Bean下的方法名，如果没有，则返回false
        System.out.println(b);
    }
~~~

## 配置文件中的占位符

1.随机数

~~~proper
{random.uuid}、{random.int}、{random.long}
~~~

2.占位符获取之前配置的值，如果没有可以用 ：指定默认值

~~~ prope
person.last-name=李四${random.uuid}
person.age=${random.int}
person.brith=2019/12/12
person.boss=false
person.maps.k1=v1
person.maps.k2=v2
person.lists=a,b,c
person.dog.name=${person.hello:hello}狗 #没有${person.hello:hello},默认为hello
person.dog.age=2
~~~

## 多环境支持Profile

### 1.多个Profile文件

默认使用application.properties,在主配置文件中编写，文件名可以是 application-{profile}.properties/yml。

在resources中新建一个application-dev.properties文件，一个application-prod.properties文件

~~~ pro
#application.properties
server.port=8081
spring.profiles.active=dev  #指定使用dev的配置
~~~



### 2.yml支持多文档块方式

文档块模式：  ---

~~~yml
server:
  port: 8081
spring:
  profiles:
    active: prod  #指定激活prod
---
server:
  port: 8082
spring:
  profiles: dev
---
server:
  port: 8083
spring:
  profiles: prod

~~~



### 3.激活指定的profile

#### 1.在配置文件中指定 

spring.profiles.active=dev

#### 2.命令行： 

--spring.profiles.active=dev  

![1575596348697](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575596348697.png)

![1575596487370](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575596487370.png)

#### 3.虚拟机参数

-Dspring.profiles.active=dev

![1575596759793](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575596759793.png)

## 配置文件的加载优先级

![1575597434647](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575597434647.png)

spring.config.location=E：/application.properties       (配置文件在本地的位置)

不需要重新打包，重新部署。当配置文件需要修改，只要在写一个application.properties，然后用spring.config.location方法就能修改配置文件。



## 外部配置环境

有很多种方式，最常用的是：

将项目打包后放到一个文件中，然后再该文件中创建一个application.properties。在该配置文件编写新的配置，然后再控制台中运行该项目，会发现原配置文件被修改了。java -jar 项目名称

# 自动配置原理@EnableAutoConfiguration

SpringBoot启动的时候，加载主配置类的@SpringBootApplication，开启**@EnableAutoConfiguration**

- @EnableAutoConfiguration作用：

  利用 AutoConfigurationImportSelector 给容器中导入一些组件。通常会自动根据你的类路径和你的bean定义自动配置，将类路径下 META-INF/spring.factories里面配置的所有EnableAutoConfiguration的值加入到容器中，每一个xxxAutoConfiguration类都是容器中的一个组件，都加入到容器中，用他们做自动配置；

  **让 Spring Boot 根据类路径中的 jar 包依赖为当前项目进行自动配置**

  

- @Configuration作用：

  表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件。

- @Conditional作用：必须@Conditional指定的条件成立，才能给容器中添加组件，配置里面的内容才能生效；

  

  自动配置类必须在一定条件下才能生效；可以通过在appliacation.properties里面启动 debug=true属性，来让控制台自动打印生效的配置。

   	Spring Boot 还会自动扫描 **@SpringBootApplication** 所在类的同级包以及下级包里的 Bean 



1、@Configuration：提到@Configuration就要提到他的搭档@Bean。使用这两个注解就可以创建一个简单的spring配置类，可以用来替代相应的xml配置文件。

@Configuration的注解类标识这个类可以使用Spring IoC容器作为bean定义的来源。@Bean注解告诉Spring，一个带有@Bean的注解方法将返回一个对象，该对象应该被注册为在Spring应用程序上下文中的bean。

2、@EnableAutoConfiguration：能够自动配置spring的上下文，试图猜测和配置你想要的bean类，通常会自动根据你的类路径和你的bean定义自动配置。

3、@ComponentScan：会自动扫描指定包下的全部标有@Component的类，并注册成bean，当然包括@Component下的子注解@Service,@Repository,@Controller。

**XxxxAutoConfiguration类**的含义是：自动配置类，目的是给容器中添加组件

**XxxxProperties类**的含义是：封装配置文件中相关属性；

![1575605636926](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575605636926.png)

Spring Boot启动的时候会通过@EnableAutoConfiguration注解找到META-INF/spring.factories配置文件中的所有自动配置类，并对其进行加载，而这些自动配置类都是以AutoConfiguration结尾来命名的，它实际上就是一个JavaConfig形式的Spring容器配置类，它能通过以Properties结尾命名的类中取得在全局配置文件中配置的属性如：server.port，而XxxxProperties类是通过@ConfigurationProperties注解与全局配置文件中对应的属性进行绑定的。

  



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



# 日志

## 1.日志框架

日志门面（日志的抽象类）：SLF4j、~~JCL~~

日志实现：Logback、~~Log4j~~;

SpringBoot:底层Spring框架，Spring框架默认是JCL；

**SpringBoot选用SLF4和logback;**

## 2.SLF4j使用

每一个日志的实现框架都有自己的配置文件。使用slf4j以后，配置文件还是做成日志实现框架自己的配置文件。

遗留问题：日志记录很多，所以要统一日志记录。

- 1.将系统中其它日志框架先排除出去
- 2.用中间包替换原有的日志框架
- 3.导入slf4j，用它来实现

## 3.SpringBoot日志关系

在pom.xml点击右键，操作如下图所示，可以显示整个SpringBoot的依赖关系。

![1575631907549](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575631907549.png)

SpringBoot使用它来做日志功能:

~~~java
 <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
      <version>2.2.1.RELEASE</version>
      <scope>compile</scope>
    </dependency>
~~~

总结：SpringBoot底层也是使用slf4j+logback的方式进行日志记录；SpringBoot也把其它的日志都替换成了slf4j;

​	  如果我们要引入其它框架，一定要把这个框架的默认日志依赖移除掉。

​	 SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其它框架的时候，只需要把这个框架依赖的日志框架排除掉。

## 4.输出日志

yaml和properties互转：<https://www.toyaml.com/index.html>

在**测试类**里面编写：

~~~java
  //记录器
    Logger logger = LoggerFactory.getLogger(getClass());

   @Test
    public void contextLoads(){
       //日志的级别
       //由低到高   trace < debug < info < warn < error
       //可以调整输出的日志级别；日志就只会在这个级别以后的高级别生效；
      logger.trace("这是trance日志...");
      logger.debug("这是debug日志...");
      //springboot默认使用的是info级别的，没有指定级别的，就用默认的级别；
      logger.info("这是info日志...");
      logger.warn("这是warn日志...");
      logger.error("这是error日志...");
   }
~~~

在**application.properties**里面配置：

~~~properties
#更改默认的级别
logging.level.com.hc=trace

# 在当前项目下生成springboot.log日志；也可以在在完整的路径下生成该日志。
#logging.file=springboot.log
#logging.file=E:/springboot.log

#在项目的当前磁盘(该项目放在E盘，所以在E盘)下的根路径下创建spring文件夹和里面的log文件夹；使用 spring.log作为默认文件
logging.path=/spring/log

#在控制台输出的日志的格式 %d 日期；%thread 线程名；%-5level  级别从左显示5个字符宽度；
# %logger{50}  表示logger名字最长的50个字符，否则按照句点分割；%msg 日志消息； %n 换行；
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
#在指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} == [%thread] == %-5level == %logger{50} == %msg%n
~~~

## 5.日志的使用

给类路径下放上每个日志框架自己的配置文件即可；SpringBoot就不使用其它默认配置了；

![1575642876239](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575642876239.png)

logback.xml:直接就被日志框架识别了；

**logback-spring.xml**:日志框架就不直接加载日志的配置项，有SpringBoot解析日志配置，可以使用SpringBoot的高级Profile功能。

# Web开发

## 1.简介

使用springboot;

- **创建SpringBoot应用，选中我们需要的模块**
- **SpringBoot已经默认将这些场景配置好了，自需要在配置文件中指定少量配置就可以运行起来**
- **自己编写业务代码**

## 2.SpringBoot对静态资源的映射规则

www.webjars.org

所有/webjars/**,都去classpath:META-INF/resources/webjars/找资源

webjars:以jar包的方式引入静态资源；

![1575645841866](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575645841866.png)

**访问jquery.js:** localhost:8080/webjars/jquery/3.4.1/jquery.js

~~~xml
  <!--引用jQuery-webjars-->在访问的时候只需要写webjars下面资源的名称即可
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.4.1</version>
        </dependency>
~~~

"/"**访问当前项目的任何资源**，（ 静态资源的文件夹）

~~~xml
"classpath:/META-INF/resources/",
"classpath:/resources/",
"classpath:/static/",
"classpath:/public/",
"/" 当前项目的根目录下
~~~

localhost:8080/abc  ===去静态资源文件夹里面找abc

localhost:8080/       === 找 index.html

**/favicon.ico 都是在静态资源文件下找

**idea 给spring boot项目设置前台html修改后立即生效**：<https://blog.csdn.net/banjing_1993/article/details/80860508>

## 3.模板引擎

=====》thymeleaf.md

禁用模板引擎：

​		spring.thymeleaf.cache=false

IDEA重新编译一下html:

​		crtl + F9

# 修改SpringBoot默认配置

模式：

- SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean、@Component）如果有就用用户配置的，如果没有，才自动配置；有些组件可以有多个（ViewResolver）将用户配置的和自己默认的组合起来。
- 在SpringBoot中会有非常多的xxxConfigurer帮助我们进行扩展配置
- 在SpringBoot中会有很多的xxxCustomizer帮助我们进行定制的配置

# 扩展SpringMVC

![1575687982918](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575687982918.png)



ctrl + o:打开重写列表         shift+F6 重名名

**使用WebMvcConfigurerAdapter可以用来扩展SpringMVC的功能**,即保留了所有的自动配置，也能用我们扩展的配置     

~~~java
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {
    //第一种扩展方法
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //super.addViewControllers(registry);
        registry.addViewController("/hello").setViewName("answer");
    }
	//第二种扩展方法
    //所有的WebMvcConfigurerAdapter组件都会一起起作用
    @Bean//将组件注册到容器中
    public WebMvcConfigurerAdapter webMvcConfigurerAdapter(){
        WebMvcConfigurerAdapter adapter = new WebMvcConfigurerAdapter(){
            @Override
            public void addViewControllers(ViewControllerRegistry registry){
                registry.addViewController("/").setViewName("login");
            }
        };
        return adapter;
    }
}
~~~



## spring管理的注解

1、@controller 控制器（注入服务）

2、@service 服务（注入dao）

3、@repository dao（实现dao访问）

4、@component （把普通pojo实例化到spring容器中，相当于配置文件中的<bean id="" class=""/>）

　  @Component,@Service,@Controller,@Repository注解的类，并把这些类纳入进spring容器中管理。



### 引用静态资源

如果配置了拦截器，拦截器要对静态资源放行。

先引入thymeleaf的cdn,然后用 th:href="@{url}"引用、图片用th:src="@{url}"。如下图： 

![1575858245440](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575858245440.png)

![1575858509469](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575858509469.png)



### 国际化

根据浏览器语言设置的信息切换

<https://www.bilibili.com/video/av38657363?p=35>

# 实时修改html

~~~xml
  <!--  修改html等不重启即时生效      -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
~~~



### 不需要重启就能更新html页面，如果使用thymeleaf模板引擎可能先需要禁用它的缓存

==========》html即时生效.md

参考网址：<https://blog.csdn.net/loujingxian/article/details/82153957>



## 拦截器

========》视图映射、拦截器.md

修改默认的date:

默认的是yyyy/MM/dd

~~~proper
spring.mvc.date-format=yyyy-MM-dd
~~~

![1575882164284](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575882164284.png)



# 定制错误页面

=======》自定义异常.md

1.**有模板引擎的情况下；error/状态码**；

- 【将错误页面命名为 错误状态码.html 放在模板引擎文件夹下的error文件夹下】，发生此状态码的错误就会来到 对应的页面

- 我们可以使用4xx和5xx作为错误页面的文件名来匹配这种类型的所有错误。精确的错误优先匹配

- 页面能获取的信息

  timestamp:时间戳

  status:状态码

  error:错误提示

  exception:异常对象

  message:异常信息

  errors:JSR303数据校验的错误都在这里

2.**没有模板引擎（模板引擎找不到这个错误），就在静态资源文件夹下找；**

3.**以上都没有错误页面，就默认使用springboot的错误提示页面；**



# 三大组件

## 1.Servlet

## 2.Filter

## 3.Listener





## 使用外置的servlet 原理

jar包：执行SpringBoot主类的main方法，启动ioc容器，创建嵌入式servlet容器。

war包：启动服务器，**服务器启动SpringBoot应用**SpringBootServletInitializer，启动ioc容器；



# Docker

## 1.简介

Docker是一个开源的应用容器引擎；

Docker支持将软件编译成一个镜像，然后在镜像中将各种软件做好配置，将镜像发布出去，其它使用者可以直接使用这个镜像；

运行中的这个镜像称为容器，容器启动是非常快速的。

## 2.核心概念

docker主机（Host）:安装Docker程序的机器（Docker直接安装在操作系统之上）；

docker客户端（Client）:连接Docker主机进行操作；

docker仓库（Registry）:用来保存各种打包好的软件镜像；

docker镜像（Images）:软件打包好的镜像，放在Docker仓库中；

docker容器（Container）:镜像启动后的实例称为一个容器；容器是独立运行的一个或一组应用

使用Docker步骤：

### 1)安装Docker

- 安装虚拟机

- 在虚拟机上安装docker

  ~~~shell
  1.检查虚拟机的版本
  uname -r
  2.安装docker
  yum install docker
  3.启动docker
  systemctl start docker
  4.查看docker版本号
  docker -v
  5.开机启动docker
  systemctl enable docker
  6.停止docker
  systemctl stop docker
  ~~~

  

### 2)去Docker仓库找到这个软件对应的镜像；

### 3）使用Docker命令运行这个镜像，这个镜像就会生成一个Docker容器

### 4）对容器的启动停止就是对软件的启动停止



**常用操作**    docker hub

![1576380501026](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576380501026.png)

![1576380799810](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576380799810.png)





**容器操作**

软件镜像----运行镜像----产生一个容器

~~~shell
1.搜索镜像
docker search tomcat
2.拉取镜像(下载)
docker pull tomcat
3.根据镜像启动容器
docker run --name mytomcat -d tomcat:latest
4.查看运行中的容器
docker ps
5.停止运行中的容器
docker stop 容器的id
6.查看所有的容器
docker ps -a
7.启动容器
docker start 容器id
8.删除一个容器
docker rm 容器id
9.启动一个做了端口映射的tomcat,可以一个镜像启动多个容器
docker run -d -p 8888:8080 tomcat   //如果报错，可以重启docker   systemctl restart docker
-d:后台运行
-p:将主机的端口
映射到容器的一个端口    主机端口：容器内部的端口
10.关闭防火墙
systemctl stop firewalld
11.查看容器的日志
docker logs 容器名/容器id
11.查看列表
docker images
~~~



提高 docker pull 的拉取速度

<https://blog.csdn.net/qq_41967899/article/details/88888240>

## docker映射mysql

~~~shel
//启动。没做端口映射，用不到
[root@localhost /]# docker run --name mysql01 -e MYSQL_ROOT_PASSWORD=root -d mysql
06b1980b1451404df4fd17b20edec95f1f16e8a77637ef2ad4df5dcbde02563e
//做了端口映射
[root@localhost /]# docker run -p 3306:3306 --name mysql02 -e MYSQL_ROOT_PASSWORD=root -d mysql
e4dbb37f01414f52a14300b45560915cd533a4d7d36a197e6136408251fad002

//端口映射，修改为utf-8编码
docker run -p 3306:3306 --name mysql02 -e MYSQL_ROOT_PASSWORD=root -d mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

~~~

# 整合druid&配置数据源监控

======>整合druid&配置数据源监控.md

# SpringBoot整合JPA

======>springboot-data-jpa.md

![1576483920479](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576483920479.png)

# 自定义starter

============》starter.md

- 启动器只用来做依赖导入
- 专门来写一个自动配置模块；
- 启动器依赖自动配置模块，项目中引入相应的starter就会引入启动器的所有传递依赖

## **如果编写自动配置**

~~~java
@Configuration //指定这是一个自定义配置类
@ConditionalOnXXXX  //在指定条件成立的情况下自动配置类生效
@AutoConfigurerAfter  //指定自动配置类的顺序
@Bean  //给容器中添加组件

@ConfigurationProperties  //结合相关XXXProperties类来绑定相关的配置
@EnableConfigurationProperties  //让XXXProperties生效加入到容器中

自动配置类要能加载
将需要启动就加载的自动配置类，配置在 META-INF/spring.factories
~~~



## 模式

启动器只用来做依赖导入；

专门来写一个自动配置模块；

启动器依赖自动配置模块；别人只需要引入启动器（starter）

名称-spring-boot-starter     ;自定义启动器名称