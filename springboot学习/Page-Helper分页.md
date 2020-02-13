Page-Helper分页 ：    <https://blog.csdn.net/qq_38977097/article/details/81395876>



### 1.导入依赖

```xml
<!--分页-->
<!--导入pagehelper相关依赖-->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.1.2</version>
</dependency>
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-autoconfigure</artifactId>
    <version>1.2.3</version>
</dependency>
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.3</version>
</dependency>
```



### 2.测试

~~~java
 		//设置分页条件，Parameters:pageNum 页码 pageSize 每页显示数量 count 是否进行count查询

		PageHelper.startPage(1, 3, true);
        List<SowingDate> users = this.sowingDateService.showSowingDateInformation("");

        for (SowingDate user : users) {
            System.out.println(user);
        }

 		PageInfo<SowingDate> pageInfo = new PageInfo<SowingDate>(users);
		 //打印分页信息
        System.out.println("数据总数：" + pageInfo.getTotal());
        System.out.println("数据总页数：" + pageInfo.getPages());
        System.out.println("最后一页：" + pageInfo.getLastPage());
~~~

