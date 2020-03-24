编辑器：VS Code

前端语言：Vue

学习目标：Vue基础、Vue Cli、Vue Router、Vue X

学习网站：<https://www.bilibili.com/video/av50680998?p=200>

​	换这个看：<https://www.bilibili.com/video/av48858873?p=1>

官网：<https://cn.vuejs.org/>

# 插件

插件参考：

<https://www.jianshu.com/p/83a981bb914b?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation>

Chinese:

中文（简体） 汉化 VS Code



mithril-emmet：

代码快速编写工具； 输入 ！ 就可以自动生成html页面、css 样式



Path Intellisense： 

路径智能提示插件



vetur：

代码补全功能



Code Run :

 在后台运行(js)代码，Ctrl+Alt+N



open in browser ： 

运行html页面,   Alt+B 在浏览器打开当前页面

修改打开的默认页面：<https://blog.csdn.net/weixin_42307675/article/details/89917967>



# Vue使用步骤

- 需要提供标签用于填充数据
- 引入 Vue.js 库文件
- 使用 Vue 的语法做功能
- 把 Vue 提供的数据填充到标签里面



# 入门

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">  
    <title>Document</title>
    
</head>
<body>
   

    <div id="app">
        <div>{{msg}}</div>
      </div>

      <script type="text/javascript" src="../js/vue.js"></script>
      <script type="text/javascript">
      
     var vm= new Vue({
          el:'#app',
          data: {
            msg:'Hello World'
          }
        });
      
      </script>
</body>
</html>
~~~



el ： 告诉浏览器要把数据填充到那个位置

data : 提供我们需要的数据

{{}} ： 插值表达式，将数据填充到HTML标签中；还支持基本的计算操作



代码运行原理：Vue代码-->Vue框架-->原生js代码



# ajax

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
<script src="https://cdn.staticfile.org/vue/2.4.2/vue.min.js"></script>
<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
</head>
<body>
<div id="app">
  {{ info }}
</div>
<script type = "text/javascript">
new Vue({
  el: '#app',
  data () {
    return {
      info: null
    }
  },
  mounted () {
    axios
      .post('https://www.runoob.com/try/ajax/demo_axios_post.php')
      .then(response => (this.info = response))
      .catch(function (error) { // 请求失败处理
        console.log(error);
      });
  }
})
</script>
</body>
</html>
```



# 遍历

~~~html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
</head>
<body>
<div id="app">
  <ol>
    <li v-for="site in sites">
      {{ site.name }}
    </li>
  </ol>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    sites: [
      { name: 'Runoob' },
      { name: 'Google' },
      { name: 'Taobao' }
    ]
  }
})
</script>
</body>
</html>
~~~



# 前端渲染

把数据填充到HTML标签中

**![1584757941488](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584757941488.png)**

前端渲染方式：

- 原生js拼接字符串
- 使用前端模板引擎
- 使用vue特有的模板语法



## Vue模板语法：

- 插值表达式
- 指令  <https://cn.vuejs.org/v2/api/#v-text>
- 事件绑定
- 属性绑定
- 样式绑定
- 分支循环结构

## 数据响应式

- html5 中的响应式(屏幕尺寸的变化导致样式的变化)
- 数据的响应式(数据的变化导致页面内容的变化)

![1584760190614](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584760190614.png)





常用的指令：  v-model  双向绑定

​                        v-on:click=' '          <==>        @click=' '   事件触发

​			v-bind  绑定

​			v-for 循环遍历

​			v-cloak 取消闪动

​			v-if

​			v-show



## 事件绑定

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">  
    <title>Document</title>
    
</head>
<body>
   

    <div id="app">
      <!-- v-text不会因为刷新，导致显示源码 -->
        <p v-text="num"></p>
        <button @click="handle">点击</button>

      </div>

      <script type="text/javascript" src="../js/vue.js"></script>
      <script type="text/javascript">
      
     var vm= new Vue({
          el:'#app',
          data: {
            num:0
          },
          methods:{
            handle: function(){
        
              //这里的 this 是 vue 的实例对象
              console.log(this === vm);
              this.num++;
            }

          }
        });
      
      </script>
</body>
</html>
~~~



## 样式绑定

有对象的绑定，有数组的绑定

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">  
    <title>Document</title>

    <style type="text/css">
      .active { 
          border: 1px solid red;
          width: 100px;
          height: 100px;
      }
    </style>
    
</head>
<body>
   

    <div id="app">
        <div v-bind:class="{active:isActive}"></div>
        <button v-on:click="handle">切换</button>
        
        <!-- 直接在 div 里面写样式 -->
          <div style="border: 1px solid orange;width: 100px;height: 100px;"></div>

      </div>

      <script type="text/javascript" src="../js/vue.js"></script>
      <script type="text/javascript">
      
     var vm= new Vue({
          el:'#app',
          data: {
            isActive:true
          },
          methods:{
            handle: function(){
              this.isActive= !this.isActive;
            
            }

          }
        });
      
      </script>
</body>
</html>
~~~



# 路由

## 后端路由

概念：根据不同的 用户 url请求，返回不同的内容

本质：URL 请求地址 与 服务器资源 之间的对应关系

![1584779225648](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584779225648.png)





## 前端路由

概念：根据不同的 用户事件，显示不同的页面内容

本质： 用户事件 与 事件处理函数 之间的对应关系

![1584779455961](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584779455961.png)



## 基本使用步骤

- 引入相关的库文件
- 添加路由链接
- 添加路由填充位
- 定义路由组件
- 配置路由规则并创建路由实例
- 把路由挂载到 Vue 根实例中



## 简单的路由

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Vue-router</title>
    
<!-- 第一步引入库文件 -->
    
<script src="https://cdn.staticfile.org/vue/2.4.2/vue.min.js"></script>
<script src="https://cdn.staticfile.org/vue-router/2.7.0/vue-router.min.js"></script>
</head>
<body>
    <div id="app">
            <h1>Hello App!</h1>
            <p>
                
                <!-- 第二步添加路由链接 -->
                
              <!-- 使用 router-link 组件来导航. -->
              <!-- 通过传入 `to` 属性指定链接. -->
              <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
              <router-link to="/foo">Go to Foo</router-link>
              <router-link to="/bar">Go to Bar</router-link>
            </p>
        
            <!-- 第三步添加路由填充位 -->
        
            <!-- 路由出口 -->
            <!-- 路由匹配到的组件将渲染在这里 -->
            <router-view></router-view>
    </div>

    <script>
        
       // 第四步定义路由组件

// 0. 如果使用模块化机制编程，導入Vue和VueRouter，要调用 Vue.use(VueRouter)

// 1. 定义（路由）组件。
// 可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }



// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
// 我们晚点再讨论嵌套路由。
const routes = [
    //重定向，默认 /foo
  { path: '/', redirect:'/foo' },
    
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

 // 第五步配置路由规则并创建路由实例

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes // （缩写）相当于 routes: routes
})

        var vm=new Vue({
            el:'#app',
            data:{

            },
            //第六步 把路由挂载到 Vue 根实例中
            //挂载路由实例对象
           //router:router
           router

        });
    
    </script>
    
</body>
</html>
~~~





## 嵌套的路由

template:'里面要写标签'

定义的自路由的链接使用 Tab 键上面的那个 ·  ，不是单引号 ‘

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Vue-router</title>

<script src="https://cdn.staticfile.org/vue/2.4.2/vue.min.js"></script>
<script src="https://cdn.staticfile.org/vue-router/2.7.0/vue-router.min.js"></script>
</head>
<body>
    <div id="app">
            <h1>Hello App!</h1>
            <p>
            
              <router-link to="/foo">Go to Foo</router-link>
              <router-link to="/bar">Go to Bar</router-link>
            </p>
          
            <router-view></router-view>
    </div>

    <script>

  
        const Foo = { template: '<div>foo</div>' }

        const Bar = { template: `<div>bar

                <router-link to="/bar/tab1">Go to tab1</router-link>,  
                <router-link to="/bar/tab2">Go to tab2</router-link>
                <hr/>
                <router-view />

                                    </div>`}


        const Tab1={
            template:'<h3>tab1</h3>'
        }
        const Tab2={
            template:'<h3>tab2</h3>'
        }

        const routes = [
            { path: '/', redirect:'/foo' },
        { path: '/foo', component: Foo },
            //children数组 表示子路由规则
            { path: '/bar', component: Bar,children:[
                { path: '/bar/tab1', component: Tab1 },
                { path: '/bar/tab2', component: Tab2 }
            ] }
        ]


        const router = new VueRouter({
        routes // （缩写）相当于 routes: routes
        })

        var vm=new Vue({
            el:'#app',
            data:{

            },
           router

        });
    
    </script>
    
</body>
</html>
~~~



## 动态路由

用  { path: '/foo/:id', component: Foo } 表示所以 /foo/ 的路由

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Vue-router</title>

    
<script src="https://cdn.staticfile.org/vue/2.4.2/vue.min.js"></script>
<script src="https://cdn.staticfile.org/vue-router/2.7.0/vue-router.min.js"></script>
</head>
<body>
    <div id="app">
            <h1>Hello App!</h1>
            <p>

              <router-link to="/foo/1">Go to Foo</router-link>
              <router-link to="/foo/2">Go to Foo</router-link>
              <router-link to="/foo/3">Go to Foo</router-link>
              <router-link to="/bar">Go to Bar</router-link>
            </p>

            <router-view></router-view>
    </div>

    <script>

const Foo = { template: '<div>foo ---- {{$route.params.id}}</div>' }
const Bar = { template: '<div>bar</div>' }



const routes = [
    //重定向，默认 /foo
  { path: '/', redirect:'/foo' },
    
  { path: '/foo/:id', component: Foo },
  { path: '/bar', component: Bar }
]

const router = new VueRouter({
  routes // （缩写）相当于 routes: routes
})

        var vm=new Vue({
            el:'#app',
            data:{

            },
        
           router

        });
    
    </script>
    
</body>
</html>
~~~



## 动态路由(传参)

props 的值为布尔类型

~~~html

    <script>

        const Foo = {   
                        props:['id'],//接收路由参数
                        template: '<div>foo ---- {{id}}</div>'  //使用路由参数
                    }
        const Bar = { template: '<div>bar</div>' }



        const routes = [
      
        { path: '/', redirect:'/foo' },
         //如果 props 被设置为 true，route.params 将会被设置为组件属性
        { path: '/foo/:id', component: Foo,props:true },
        { path: '/bar', component: Bar }
        ]

        const router = new VueRouter({
        routes 
        })

        var vm=new Vue({
            el:'#app',
            data:{

            },
       
           router

        });
    
    </script>

~~~

props 的值为对象类型

：id 不能去掉，充当占位符；也不能显示id的值，因为 props 对象中没有 id 属性

~~~html
<script>

        const Foo = {   
                        props:['id','name','age'],
                        template: '<div>foo ---- {{id}} ---{{name}} ----{{age}}</div>'
                    }
        const Bar = { template: '<div>bar</div>' }



        const routes = [
      
        { path: '/', redirect:'/foo' },
            
        { path: '/foo/:id', component: Foo,props:{name:'hc',age:22} },
        { path: '/bar', component: Bar }
        ]

        const router = new VueRouter({
        routes 
        })

        var vm=new Vue({
            el:'#app',
            data:{

            },
        
           router

        });
    
    </script>
~~~



props 的值为函数类型

修改这句代码，用函数类型，可以实现动态、静态传参

~~~html
 { path: '/foo/:id', component: Foo,props: route =>({name:'hc',age:22,id:route.params.id}) },
~~~

# 

## 编程式导航

~~~html
    const Foo = {   
                        props:['id','name','age'],
                        template: `<div>
                                        foo ---- {{id}} ---{{name}} ----{{age}}
                                        <button @click="goBar">goBar页面</button>
                                    </div>`,
                        methods:{
                            goBar(){
                                this.$router.push('/bar')
                            }
                        }
                    }
        const Bar = { template: `<div>
                                    bar
                                    <button @click="goBack">后退页面</button>
                                </div>`,
                       methods:{
                                    goBack(){
                                        this.$router.go(-1);
                                    }
                                    
                                }
                       
                    }
~~~





# 模块化

ES6模块化规范：

- 每个 js 文件都是一个独立的模块
- 导入模块成员 使用 import 关键字
- 暴露模块成员 使用 export 关键字



Babel就是为了实现用ES6写的脚本能够在实际的环境中运行。

![1584846281619](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584846281619.png)

ES6

<https://blog.csdn.net/qq_44997147/article/details/101164516>

webpack入门：

<https://blog.csdn.net/maolaixin/article/details/84102122?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task>

终端输入：

webpack-cli -g



# 第二个视频

源码：<https://github.com/shenghy/VueDemo>

安装开始级的web服务器：npm install -g live-server

启动服务：live-server

关闭服务： Ctrl + C ；选择 Y



初始化： npm init 会生成 package.json



主要学习思想，代码没有全部敲



v-if ：判断是否加载，可以减轻服务器的压力，在需要是加载

v-show ： 调整 css display属性，可以使客户端操作更加流畅

v-for   循环；还可以排序；<https://github.com/shenghy/VueDemo/blob/master/example/v-for.html>

v-model     .lazy  懒加载           .number 只显示数字       .trim 去空格

v-bind    绑定；可以绑定判断、数组、对象

v-pre  原样渲染

 v-cloak   页面加载，渲染完成后才显示出来

v-once    只进行一次渲染



显示选中id

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>v-model 绑定数据源</title>
</head>
<body>
    <h1>v-model 绑定数据源 案例</h1>
    <hr>
    <div id="app">
   
       <h3>多选绑定一个数组</h3>
       <p>
            <input type="checkbox" id="JSPang" value="JSPang" v-model="web_Names">
            <label for="JSPang">JSPang</label><br/>
            <input type="checkbox" id="Panda" value="Panda" v-model="web_Names">
            <label for="JSPang">Panda</label><br/>
            <input type="checkbox" id="PanPan" value="PanPan" v-model="web_Names">
            <label for="JSPang">PanPan</label>
            <p>{{web_Names}}</p>
       </p>
       <hr>
       <h3>单选按钮绑定</h3>
       <input type="radio" id="one" value="男" v-model="sex">
       <label for="one">男</label>
       <input type="radio" id="two" value="女" v-model="sex">
       <label for="one">女</label>
       <p>{{sex}}</p>
       <hr>
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
             
                web_Names:[],
                sex:'男'
            }
        })
    </script>
</body>
</html>
~~~



## Vue-Cli配置

检查是否安装node : node -v

检查是否安装 npm：  npm-v

全局安装Vue-Cli脚手架  ：  npm install vue-cli -g

检查是否安装Vue-Cli  ：  vue -V



使用webpack 模板：

vue init webpack  vuecli(小写的项目名)

​	

~~~txt
? Project name vue-demo # 项目名称，直接回车，按照括号中默认名字（注意这里的名字不能有大写字母，如果有会报错Sorry, name can no longer contain capital letters），阮一峰老师博客为什么文件名要小写 ，可以参考一下。
? Project description A Vue.js project # 项目描述,随便写
? Author # 作者名称
? Vue build standalone # 我选择的运行加编译时
	Runtime + Compiler: recommended for most users
? Install vue-router? Yes # 是否需要 vue-router，路由肯定要的
? Use ESLint to lint your code? Yes # 是否使用 ESLint 作为代码规范.
? Pick an ESLint preset Standard # 一样的ESlint 相关
? Set up unit tests Yes # 是否安装单元测试
? Pick a test runner 按需选择 # 测试模块
? Setup e2e tests with Nightwatch? 安装选择 # e2e 测试
? Should we run `npm install` for you after the project has been created? (recommended) npm

~~~

​	

cd vuecli

npm run dev



运行后，出现的配置文件

![1584898148796](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584898148796.png)



build 项目构建，webpack相关的都在这里

config 项目开发环境配置

node_modules  项目所需要的包

src 开发的目录

static 静态文件目录



~~~txt
|-- build                            // 项目构建(webpack)相关代码
|   |-- build.js                     // 生产环境构建代码
|   |-- check-version.js             // 检查node、npm等版本
|   |-- utils.js                     // 构建工具相关
|   |-- vue-loader.conf.js           // webpack loader配置
|   |-- webpack.base.conf.js         // webpack基础配置
|   |-- webpack.dev.conf.js          // webpack开发环境配置,构建开发本地服务器
|   |-- webpack.prod.conf.js         // webpack生产环境配置
|-- config                           // 项目开发环境配置
|   |-- dev.env.js                   // 开发环境变量
|   |-- index.js                     // 项目一些配置变量
|   |-- prod.env.js                  // 生产环境变量
|-- src                              // 源码目录
|   |-- components                   // vue公共组件
|   |-- router                       // vue的路由管理
|   |-- App.vue                      // 页面入口文件
|   |-- main.js                      // 程序入口文件，加载各种公共组件
|-- static                           // 静态文件，比如一些图片，json数据等
|-- .babelrc                         // ES6语法编译配置
|-- .editorconfig                    // 定义代码格式
|-- .gitignore                       // git上传需要忽略的文件格式
|-- .postcsssrc                       // postcss配置文件
|-- README.md                        // 项目说明
|-- index.html                       // 入口页面
|-- package.json                     // 项目基本信息,包依赖信息等
~~~



项目写完了，打包 npm run build ，会生成 dist 目录，可以把 dist 目录上传到服务器



## Vue-Cli使用

简单使用：

在 components 文件夹下新建Hi.vue

~~~vue
<template>

    <div>

        <h2>{{msg}}</h2>
    </div>
</template>

<script>
export default {
  name: 'Hi',
  data () {
    return {
      msg: 'hi page'
    }
  }
}
</script>

<style scoped>

</style>

~~~

修改 src 根目录下的index.js

~~~js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Hi from '@/components/Hi'   //新添加的

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },{                       //新添加的
      path: '/Hi',
      name: 'Hi',
      component: Hi
    }
  ]
})

~~~

输入 <http://localhost:8080/#/Hi>    可以显示Hi页面



修改 src 文件夹 下面的 App.vue

~~~vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
	
      <!-- 新添加的 -->
    <div>
      <router-link to="/">首页</router-link>
      <router-link to="/Hi">Hi页面</router-link>
    </div>

    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

~~~

修改App.vue后，可以点击链接就显示相应页面的内容



## 嵌套子路由

### Hi.vue

​    ======          新建 Hi1.vue   Hi2.vue，代码和 Hi.vue 一样

~~~vue
<template>

    <div>

        <h2>{{msg}}</h2>
		<!-- 子路由 在这里显示 -->
        <router-view/>
    </div>
</template>

<script>
export default {
  name: 'Hi',
  data () {
    return {
      msg: 'hi page'
    }
  }
}
</script>

<style scoped>

</style>

~~~

### App.vue

~~~vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
<!-- 新添加的 -->
    <div>
      <router-link to="/">首页</router-link>
      <router-link to="/Hi">Hi页面</router-link>
      <router-link to="/Hi1">Hi1页面</router-link>
      <router-link to="/Hi2">Hi2页面</router-link>
    </div>

    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

~~~

### index.js

~~~js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Hi from '@/components/Hi'
import Hi1 from '@/components/Hi1'
import Hi2 from '@/components/Hi2'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },{
      path: '/Hi',
      name: 'Hi',
      component: Hi,
      // 添加子路由路径
      children:[
        {path:"/",component:Hi},
        {path:"/Hi1",component:Hi1},
        {path:"/Hi2",component:Hi2}
      ]
    }
  ]
})

~~~



第二天重新打开项目报错：{ parser: "babylon" } is deprecated; we now treat it as { parser: "babel" }.

解决：<https://blog.csdn.net/u011280778/article/details/88107472>

但是 npm run dev 还是有报错，所以重新创建了一个 vue-cli 项目



## 动态路由传参

### params.vue

~~~vue
<template>
    <div>
        <h2>{{msg}}</h2>
        newsId: {{$route.params.newsId}}
        newsTitle: {{$route.params.newsTitle}}
    </div>
</template>

<script>
export default {
    data(){
        return{
            msg:'params page'
        }
    }
}
</script>
<style>

</style>

~~~

### index.js

配置路由规则表

~~~js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Params from '@/components/params'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {
      path: '/params/:newsId/:newsTitle',
      component: Params
    }
  ]
})

~~~

### App.vue

页面入口文件

~~~vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
     <router-link to="/">Home</router-link>
    <router-link to="/params/1/xinwen">动态路由传参</router-link>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

~~~



## 重定向

redirect 重定向到 /

### App.vue

~~~vue
//新添加的  		
<router-link to="/goHome">goHome</router-link>
      <router-link to="/goParams/2/hc">goParams</router-link>
~~~



### index.js

~~~js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Params from '@/components/params'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {
      path: '/params/:newsId/:newsTitle',
      component: Params
    },{
      path:'/goHome',
      redirect:'/'
    },,{
      path:'/goParams/:newsId/:newsTitle',
      redirect:'/params/:newsId/:newsTitle'
    }
  ]
})


~~~

## 别名

index.js

~~~js
{
      path:'/',
      component: HelloWorld,
      alias:'/hc'
    }
~~~

App.vue

~~~vue
router-link to="/hc">hc</router-link>
~~~



## 淡入淡出 

<https://www.bilibili.com/video/av48858873?p=38>



## Mode去掉url里面的 #

index.js



~~~js
mode:'history',
routes: [....]
~~~

## 404页面

### index.js

~~~js
import Error from '@/components/Error'

.....

{
      path:'*',
      component:Error
    }
~~~

### Error.vue

~~~vue
<template>
    <div>
        <h2>{{msg}}</h2>
       
    </div>
</template>

<script>
export default {
    data(){
        return{
            msg:'Error 404'
        }
    }
}
</script>
<style>

</style>

~~~

输入错误url，会显示出 404页面



## 钩子函数

params.vue

~~~vue
<template>
    <div>
        <h2>{{msg}}</h2>
        newsId: {{$route.params.newsId}}
        newsTitle: {{$route.params.newsTitle}}
    </div>
</template>

<script>
export default {
    data(){
        return{
            msg:'params page'
        }
    },
    beforeRouteEnter:(to,from,next) =>{
        console.log("准备进入 params 路由模板");
        next();
    },
    beforeRouteLeave:(to,from,next) =>{
        console.log("准备离开 params 路由模板");
        next();
    }
}
</script>
<style>

</style>

~~~

在进入 params.vue 之前会先执行 beforeRouteEnter，离开params.vue后，会执行beforeRouteLeave

## 编程式导航

~~~vue
<template>
    <div>
        <h2>{{msg}}</h2>
        <button @click="goHome"></button>
    </div>
</template>

<script>
export default {
    data(){
        return{
            msg:'params page'
        }
    },
    methods:{
        goHome(){
            this.$router.go(-1);
        }
    }
}
</script>
<style>
button{
    height:50px;
    width:50px;
}
</style>

~~~

在本页面，不用路由，可以直接跳转到其他页面



## Vue X

安装vuex ：     npm install vuex --save

### 简单的使用

index.js中新增的路由

~~~js
import Count from '@/components/count'

{
      path:'/count',
      component:Count
    }
~~~

新建vuex文件夹，创建store.js

~~~js
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);

const state={
    count:1
}

const mutations={
    add(state){
        state.count++;
    },
    reduce(state){
        state.count--;
    }
}

export default new Vuex.Store({
    state,
    mutations
})


~~~

在components文件夹下创建Count.vue

~~~vue

<template>
    <div>
        <h2>{{msg}}</h2>
        <h3>{{$store.state.count}}</h3>
        <p>

            <button @click="$store.commit('add')">+</button>
            <button @click="$store.commit('reduce')">-</button>
        </p>
    </div>
    
</template>

<script>
import store from '@/vuex/store';

export default {
    data(){
        return {
            msg:'hello vuex'
        }
    },
    store
}
</script>

~~~

