

# 添加依赖

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
~~~

# 使用前提

~~~ht
<html xmlns:th="http://www.thymeleaf.org">
~~~





# Thymeleaf常用th标签

<https://blog.csdn.net/fulq1234/article/details/85003273>



显示作用域中的值：<https://www.cnblogs.com/ynhwl/p/10016957.html>



```html
<img class="img-responsive" alt="如果无法显示图像，浏览器将显示替代文本" th:src="@{/images/1.jpg}" />

<form action="subscribe.html" th:action="@{/subscribe}">  //th:action优先级高于普通的action
    
 <a th:href="@{/login}" th:unless=${session.user != null}>Login</a>   //链接地址
   
 <td th:onclick = "'getCollect()'"></td>  //点击事件



```

变量表达式 ${... ...}

~~~html
<input type="text" name="userName" value="James Carrot" th:value="${user.name}" /> //引用user对象的name属性值
~~~



# 公共页面抽取

~~~html
1.被抽取的top.html页面的公共部分
 	<footer th:fragment="copy">
        这是一段要被引用的文字
    </footer>

	<div id="aaa">
        根据id引入的片段。
    </div>

2.引用的页面
//top::copy  表示引用top.html里面的名字为copy的
    <div th:insert="top::copy"></div>
    <div th:replace="top::copy"></div>
    <div th:include="top::copy"></div>

<div th:insert="top::#aaa"></div>  //想要那个页面的内容，就用 页面名+该页面想要的内容的id名 来获取
~~~

由图可以看出

![1575879702409](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575879702409.png)

th:insert：将公共片段整个插入到声明引入的元素中

th:replace：将声明引入的元素替换为公共元素

th:include：将被引入的片段的内容包含进了这个标签



# th:each遍历

![1575880926219](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575880926219.png)



![1575880860877](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575880860877.png)





![1575882499035](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575882499035.png)

![1575882519634](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1575882519634.png)



![1577698120370](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1577698120370.png)



# 项目中经常使用的thymeleaf

## 获取session值

~~~html
 <div th:text="${session.loginUser}">[...]</div> 
<div th:text="${session.login.name}">[...]</div> //session对象中的某一个属性值
~~~









