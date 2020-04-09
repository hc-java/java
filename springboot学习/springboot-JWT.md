参考博客：



SpringBoot整合JWT

<https://www.jianshu.com/p/9f5b09b3739a>

<https://www.jianshu.com/p/e88d3f8151db>



JWT中token在前端的操作：

- 先将 token 存到本地sessionStorage、localStorage

- 前端访问后台时，要在ajax里面的header中添加 token

  ~~~vue
  		 axios({
                  method: 'get',
                  url: 'http://localhost:8081/getMessage',
                  headers : {
                      token:window.sessionStorage.getItem("mytoken")
                  }
              });
  ~~~

  







![1586077112678](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1586077112678.png)

  1.前端输入用户名、密码 到后台登录

  2.后端根据用户ID生成Token、返回给前端

  3.前端ajax请求、通过header头部设置 Authorization ：token

  4.后端通过Filter、拦截所有请求、处理请求是否合法（token失效、token为空、token过期）

