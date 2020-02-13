# 基础知识

**ng-app** 指令初始化一个 AngularJS 应用程序。

**ng-init** 指令初始化应用程序数据。

**ng-model** 指令把元素值（比如输入域的值）绑定到应用程序。

AngularJS 参考手册:<https://www.runoob.com/angularjs/angularjs-reference.html>



**ng-controller** 指令定义了应用程序控制器。

## 按钮

~~~html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="https://cdn.staticfile.org/angular.js/1.4.6/angular.min.js"></script>
</head>
<body>

<div ng-app="myApp" ng-controller="myCtrl">
	<input ng-model="name">
	<h1>{{greeting}}</h1>
	<button ng-click='sayHello()'>点我</button>	
</div>

<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.name = "Runoob";
	$scope.sayHello = function() {
    	$scope.greeting = 'Hello ' + $scope.name + '!';
	};
});
</script>

<p>当你修改输入框中的值时，会影响到模型(model),当然也会影响到控制器对应的属性值。</p>

</body>
</html>
~~~





# 路由

~~~html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>AngularJS 路由实例 - 菜鸟教程</title>
    <script src="https://cdn.staticfile.org/angular.js/1.7.0/angular.min.js"></script>
    <script src="https://cdn.staticfile.org/angular.js/1.7.0/angular-route.min.js"></script>
</head>
<body ng-app='routingDemoApp'>

<h2>AngularJS 路由应用</h2>
<ul>
    <li><a href="#!/main">main</a></li>
    <li><a href="#!/index">index</a></li>
</ul>

<div ng-view></div>
<script>
    angular.module('routingDemoApp',['ngRoute'])
        .config(['$routeProvider', function($routeProvider){
            $routeProvider
                .when('/main', {
                templateUrl: 'main',
            })
                 .when('/index', {
                templateUrl: 'index',
            })
                .otherwise({redirectTo:'/main'});
        }]);

</script>
</body>
</html>
~~~

~~~java
    
	//angular测试
    @GetMapping("/angular")
    public String Angular(){
        System.out.println("angular is coming...");
        return "angular";
    }
	//main测试
    @GetMapping("/main")
    public  String Main(){
        System.out.println("main is coming...");
        return "main";
    }
	//index测试
    @GetMapping("/index")
    public  String Index(){
        System.out.println("main is coming...");
        return "index";
    }
~~~

小结：springboot下的templates无法直接访问里面的静态页面，必须通过服务器内部进行访问，也就是要走控制器  ->  服务  ->  视图解析器这个流程才行

​	 templates文件夹，是放置模板文件的，因此需要视图解析器来解析它



​	如果放在static文件夹下，可以直接访问静态页面，不走控制器。templateUrl：'页面名.html'

# 表单

~~~html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="https://cdn.staticfile.org/angular.js/1.4.6/angular.min.js"></script>
</head>
<body>

<div ng-app="myApp" ng-controller="formCtrl">
  <form novalidate>
    First Name:<br>
    <input type="text" ng-model="user.firstName"><br>
    Last Name:<br>
    <input type="text" ng-model="user.lastName">
    <br><br>
    <button ng-click="reset()">RESET</button>
  </form>
  <p>form = {{user}}</p>
  <p>master = {{master}}</p>
</div>
 
<script>
var app = angular.module('myApp', []);
app.controller('formCtrl', function($scope) {
    $scope.master = {firstName: "John", lastName: "Doe"};
    $scope.reset = function() {
        $scope.user = angular.copy($scope.master);
    };
    $scope.reset();
});
</script>
	
</body>
</html>
~~~



# ajax

~~~html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <script src="https://cdn.staticfile.org/angular.js/1.6.3/angular.min.js"></script>
</head>
<body>

<div ng-app="myApp" ng-controller="siteCtrl">

    <ul>
        <li ng-repeat="x in names">
            {{x}}
        </li>
    </ul>

<!--    {{names}}-->
<!--    {{names.name}}-->
</div>

<script>
    var app = angular.module('myApp', []);
    app.controller('siteCtrl', function($scope, $http) {
        var params={params:{"id":3}};//传参数

        $http.get("main2",
                    params
                  )
            .then(function (response) {
                 $scope.names = response.data;

                // console.log($scope.names);
                // console.log(response.data);
            });
    });
</script>

</body>
</html>
~~~



~~~java
   @GetMapping("/main2")
    @ResponseBody
    public  String Main2(Model model,String id){
        System.out.println("main2222 is coming...");
        System.out.println(id);
        model.addAttribute("name","hc");
        model.addAttribute("sex","boy");
        model.addAttribute("age","22");
        Gson gson = new Gson();
        String sites = gson.toJson(model);
        System.out.println(sites);
        return sites;
    }
~~~



# 遍历

**ng-repeat** 指令会重复一个 HTML 元素（对象数组）。

~~~html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="https://cdn.staticfile.org/angular.js/1.4.6/angular.min.js"></script> 
</head>
<body>

<div ng-app="" ng-init="names=[
{name:'Jani',country:'Norway'},
{name:'Hege',country:'Sweden'},
{name:'Kai',country:'Denmark'}]">

<p>循环对象:</p>
<ul>
  <li ng-repeat="x in names">
  {{ x.name + ', ' + x.country }}</li>
</ul>

</div>

</body>
</html>

~~~

