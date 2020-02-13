

# 自定义场景启动器starter

## 1.创建一个空项目

 

![1576717383402](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576717383402.png)

![1576712829916](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576712829916.png)

## 2.创建一个maven

做为starter启动器

![1576712901572](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576712901572.png)

![1576712967862](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576712967862.png)

![1576713007029](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576713007029.png)



## 3.创建一个Spring Initializr

做为自动配置类

![1576713089506](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576713089506.png)

![1576713205601](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576713205601.png)

![1576713246476](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576713246476.png)





starter启动器：只做依赖引入

从自动配置的pom.xml里面复制    给starter启动器中

~~~xml
<groupId>com.hc.starter</groupId>
	<artifactId>hc-spring-boot-starter-autoconfigurer</artifactId>
	<version>0.0.1-SNAPSHOT</version>
~~~



自动配置类 删除了，resources和java下的主程序类；test文件也删掉；pom.xml文件只留下 spring-boot-starter 的依赖。



# start启动器代码

## pom.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!--从自动配置类里面拿的代码片段 -->
    <groupId>com.hc.starter</groupId>
    <artifactId>hc-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!--启动器-->
    <dependencies>
<!--    引入自动配置    -->
        <dependency>
            <groupId>com.hc.starter</groupId>
            <artifactId>hc-spring-boot-starter-autoconfigurer</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
    </dependencies>
    
<!--不加会报错：Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

</project>
~~~



# 自动配置类代码

## pom.xml

里面注释的代码，需要删除或注释；不然会报错

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.2.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.hc.starter</groupId>
	<artifactId>hc-spring-boot-starter-autoconfigurer</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>hc-spring-boot-starter-autoconfigurer</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<!--引入spring-boot-starter；所有starter的基本配置-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

<!--		<dependency>-->
<!--			<groupId>org.springframework.boot</groupId>-->
<!--			<artifactId>spring-boot-starter-test</artifactId>-->
<!--			<scope>test</scope>-->
<!--			<exclusions>-->
<!--				<exclusion>-->
<!--					<groupId>org.junit.vintage</groupId>-->
<!--					<artifactId>junit-vintage-engine</artifactId>-->
<!--				</exclusion>-->
<!--			</exclusions>-->
<!--		</dependency>-->
	</dependencies>

	<build>
<!--		<plugins>-->
<!--			<plugin>-->
<!--				<groupId>org.springframework.boot</groupId>-->
<!--				<artifactId>spring-boot-maven-plugin</artifactId>-->

<!--			</plugin>-->
<!--		</plugins>-->
	</build>

</project>

~~~

## HelloService

~~~java
package com.hc.starter;

public class HelloService {

    HelloProperties helloProperties;

    public HelloProperties getHelloProperties() {
        return helloProperties;
    }

    public void setHelloProperties(HelloProperties helloProperties) {
        this.helloProperties = helloProperties;
    }

    public String sayHello(String name){
        return  helloProperties.getPrefix()+"--"+name +"--"+helloProperties.getSuffix();
    }
}

~~~

## HelloProperties

~~~java
package com.hc.starter;

import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties(prefix = "hc.hello")
public class HelloProperties {
    private  String prefix;
    private String suffix;

    public String getPrefix() {
        return prefix;
    }

    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }

    public String getSuffix() {
        return suffix;
    }

    public void setSuffix(String suffix) {
        this.suffix = suffix;
    }
}

~~~

## HelloServiceAutoConfiguration

~~~java
package com.hc.starter;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@ConditionalOnWebApplication//web应用才生效
@EnableConfigurationProperties(HelloProperties.class)
public class HelloServiceAutoConfiguration {

    @Autowired
    HelloProperties helloProperties;
    @Bean
    public HelloService helloWorld() {
        HelloService helloService = new HelloService();
        helloService.setHelloProperties(helloProperties);
        return helloService;

    }
}

~~~

## spring.factories

spring.factories要在resources目录下的META-INF下。

具体配置-参考：

![1576718385728](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576718385728.png)

~~~factories
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.hc.starter.HelloServiceAutoConfiguration #自己的自动配置路径
~~~



## 安装到maven仓库中

启动器 和 自动配置类都要install

![1576719387743](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576719387743.png)

# 测试代码

## 创建一个web项目

## pom.xml

~~~xml
    <!--引入自定义的starter-->
        <dependency>
            <groupId>com.hc.starter</groupId>
            <artifactId>hc-spring-boot-starter</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
~~~



## HelloController

~~~java
package com.hc.springboot.controller;

import com.hc.starter.HelloService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    //引用自己写的HelloService
    @Autowired
    HelloService helloService;

    @GetMapping("/hello")
    public  String hello() {
        return  helloService.sayHello("hchc");
    }

}

~~~

## application.properties

~~~properties
hc.hello.prefix=HC
hc.hello.suffix=HELLO World!

~~~



# 测试结果

![1576720614610](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576720614610.png)



![1576721632606](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576721632606.png)

# 总结

场景启动器starter将自己写好的代码，通过自动配置放到maven仓库中。其它的项目都可以从仓库中引用这个starter。

优点：如果事先写好了大量的starter，开发就会很简单，只要引用就可以了。

缺点：如果只是个小项目，实现starter划不来，因为自己写的也可以被自己项目直接使用，不用install到maven仓库中。