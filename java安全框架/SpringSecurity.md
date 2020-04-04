视频学习链接：

<https://www.bilibili.com/video/BV1Et411Y7tQ?p=99>



# 创建一个SpringBoot项目





# 认证、授权

## MySecurityConfig



~~~java
package com.example.security.config;

import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

@EnableWebSecurity  //这是个组合注解，里面有 @Configuration 注解
public class MySecurityConfig extends WebSecurityConfigurerAdapter {

    //定义授权规则
    @Override
    protected void configure(HttpSecurity http) throws Exception {
       // super.configure(http);
        //定制请求的授权规则
        http.authorizeRequests().antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("VIP1")
                .antMatchers("/level2/**").hasRole("VIP2")
                .antMatchers("/level3/**").hasRole("VIP3");
        //开启自动配置的登录功能;如果没有登录，没有权限就会来到登陆页面
        http.formLogin();
        //1. /login来到登陆页（）
        //2. 重定向来到 /login?error
        //3. ...

        //开启自动配置的注销功能
        http.logout().logoutSuccessUrl("/");// 注销成功，来到 / 页面
        //1. 访问 /logout 表示用户注销，清空 session
        //2. 注销成功会返回 /login?logout 页面

    }

    //定义认证规则
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //super.configure(auth);
       /* 因为Spring boot 2.0.3引用的security 依赖是 spring security 5.X版本，
           此版本需要提供一个PasswordEncorder的实例，否则后台汇报错误：
            java.lang.IllegalArgumentException: There is no PasswordEncoder mapped for the id "null"
            解决：https://www.cnblogs.com/chenyanlong/p/10688662.html
        */

        auth.inMemoryAuthentication()
                .passwordEncoder(new MyPasswordEncoder())
                .withUser("admin").password("123").roles("VIP1","VIP2")
                .and()
                .withUser("aaa").password("123").roles("VIP2","VIP3")
                .and()
                .withUser("bbb").password("123").roles("VIP3","vip1");


    }
}

~~~

## Controller

~~~java
package com.example.security.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class TestController {

    @RequestMapping("/")
    public String index(){
        return "index";
    }

    @RequestMapping("/login")
    public String login(){
        return "login";
    }

    @RequestMapping("/loginIn")
    public String loginIn(){
        return "main";
    }

    @RequestMapping("/level1/{path}")
    public String level1(@PathVariable("path")String path){
        return "level1/"+path;
    }

    @RequestMapping("/level2/{path}")
    public String level2(@PathVariable("path")String path){
        return "level2/"+path;
    }

    @RequestMapping("/level3/{path}")
    public String level3(@PathVariable("path")String path){
        return "level3/"+path;
    }
}

~~~

## index

~~~html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org" >
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>


<h2 align="center"><a th:href="@{/login}">登录</a></h2>
<form th:action="@{/logout}" method="post">
    <input type="submit" value="注销">
</form>

<h3>VIP1可以访问</h3>
<ul>
    <li><a th:href="@{level1/1}">VIP1-1</a></li>
    <li><a th:href="@{level1/2}">VIP1-2</a></li>
    <li><a th:href="@{level1/3}">VIP1-3</a></li>
</ul>

<h3>VIP2可以访问</h3>
<ul>
    <li><a th:href="@{level2/1}">VIP2-1</a></li>
    <li><a th:href="@{level2/2}">VIP2-2</a></li>
    <li><a th:href="@{level2/3}">VIP2-3</a></li>
</ul>

<h3>VIP3可以访问</h3>
<ul>
    <li><a th:href="@{level3/1}">VIP3-1</a></li>
    <li><a th:href="@{level3/2}">VIP3-2</a></li>
    <li><a th:href="@{level3/3}">VIP3-3</a></li>
</ul>



</body>
</html>
~~~

