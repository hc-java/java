# 使用IDEA搭建一个简单的SpringBoot项目——详细过程

<https://blog.csdn.net/baidu_39298625/article/details/98102453>



## 1.快速创建一个SpringBoot项目

## 2.pom.xml配置

如果创建项目的时候，没有选择web、thymeleaf、mysql、mybatis；就要自己添加dependency

mysql要注意版本号，过高会无法启动；

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
    <groupId>com.hc</groupId>
    <artifactId>springboot-web</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot-web</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <!--引用web模块-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--引用jQuery-->
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.4.1</version>
        </dependency>
        <!--引用thymeleaf-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <!-- mybatis -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.1.1</version>
        </dependency>
        <!--  jdbc      -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <!-- mysql -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.41</version>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

~~~

## 3.application.yml配置

~~~xml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/hotel?useUnicode=true&characterEncoding=utf8
    username: root
    password: root


mybatis:
  mapper-locations: classpath:mapper/*.xml  #配置映射文件
  type-aliases-package: com.hc.bean #配置实体类
~~~

~~~proper

//application.properties:
#项目访问路径
spring.context-path=/项目名
#修改默认端口
server.port=8081
#Tomcat的设置
server.tomcat.xxx
~~~



## 4.SpringbootWebApplication程序入口

~~~java

@SpringBootApplication
@MapperScan("com.hc.mapper")//扫描mapper下所有的接口
public class SpringbootWebApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootWebApplication.class, args);
    }

}

~~~

## 5.连接数据库

![1575712733158](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575712733158.png)

![1575712800239](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575712800239.png)

如果点击“Test Connection”失败，就点击![1575712947726](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575712947726.png)，选择新的驱动

![1575712904350](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575712904350.png)

## 6.内容

和以前的SSM框架一样写代码。



修改dao报错，不影响使用。

![1575772431827](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575772431827.png)

## 7.前端页面

~~~html
 
 //element 的javascript
 <script type="text/javascript">
 	 new Vue({
        el:'#app',
        data:{

        },
        methods:{

        },
        mounted: function () {

        }
    })
 </script>

~~~



laui:

<https://www.layui.com/demo/grid.html>