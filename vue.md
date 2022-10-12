[TOC]

前后端分离的项目，路飞学城  vue  + drf(djangorestframework后端接口)  开发项目

crm项目， 非前后端分离    index.html  {{ msg }}  --  views.py def index(request):render(request,'index.html',{'msg':'xxoo'})    /index/     

Vue 前端框架   MVVM -- model -- view -- viewmodel      ---  template模板渲染

model--数据

view -- 视图   -- html标签   类似于jquery  $('#d1').text('xxx')



viewmodel：数据驱动视图 vue的核心，model数据直接驱动到视图中(html标签中)

django  -- MTV模式   model  template  view + url控制器    template这一块功能就去掉了

view--视图：  后端逻辑  FBV和CBV



前端  ECMAScript5 -- es5

vue多数用的都是es6语法



# 前戏

## es6的基本语法

```js
let：
特点：   
1.a是局部作用域的function xx(){let a = 'xxoo';}  if(){let a = 'ss';} 
2.不存在变量提升  
3.不能重复声明（var可以重复声明）  ,var声明的不能用let再次声明,反之也是
4 let声明的全局变量不从属于window对象，var声明的全局变量从属于window对象。

关于第4个特点的简单说明：
	ES5声明变量只有两种方式：var和function。
	ES6有let、const、import、class再加上ES5的var、function共有六种声明变量的方式。
	还需要了解顶层对象：浏览器环境中顶层对象是window.
	ES5中，顶层对象的属性等价于全局变量。var a = 10; window.a 
	ES6中，有所改变：var、function声明的全局变量，依然是顶层对象的属性；let、const、class声明的全局变量不属于顶层对象的属性，也就是说ES6开始，全局变量和顶层对象的属性开始分离、脱钩，目的是以防声明的全局变量将window对象的属性造成污染，因为window对象是顶层对象，它里面的属性是各个js程序中都可以使用的，不利于模块化开发，并且window对象有很多的自有属性，也为了防止对window的自由属性的污染，所以在ES6中将顶层对象和全局变量进行隔离。
	举例：
		var a = 1;
		console.info(window.a);  // 2
		
		let b = 2;
		console.info(window.b); //undefined

// var a;  -- undefined
console.log(a); -- undefined
var a = 10;

var b = '景顺';
console.log(c);  -- 报错
let c = '谢景顺'；


const ：特点：  1.局部作用域  2.不存在变量提升  3.不能重复声明  4.一般声明不可变的量
const pi = 3.1415926;
//pi = 'xx' -- 报错

模板字符串：tab键上面的反引号，${变量名}来插入值  类似于python的三引号 """adfadsf"""，可以写多行文本
另外还可以通过它来完成字符串格式化
示例：
	let bb = 'jj';
	var ss = `你好${bb}`;
  console.log(ss); -- '你好jj'
```







## es5和es6的函数对比

```js
    //ES5写法
    function add(x){
        return x
    }
    add(5);
    //匿名函数
    var add = function (x) {
        return x
    };
		add(5);
    //ES6的匿名函数
    let add = function (x) {
        return x
    };
    add(5);
    //ES6的箭头函数,就是上面方法的简写形式
    let add = x => {
        console.log(x);
        return x
    };
    console.log(add(20));
    //更简单的写法，但不是很易阅读
    let add = x => x;
    console.log(add(5));
    多个参数的时候必须加括号，函数返回值还是只能有一个，没有参数的，必须写一个()
    let add = (x,y) => x+y;
```



## 自定义对象中封装函数的写法

```js
   //es5对象中封装函数的方法
    var name = '子俊';
    var person1 = {
        name:'超',
        age:18,
        f1:function () {  //在自定义的对象中放函数的方法
            console.log(this);
            console.log(this.name)
        }
    };  

//es5对象中封装函数的方法
    var name = '子俊';
    var person1 = {
        name:'超',
        age:18,
        f1:function () {  //在自定义的对象中放函数的方法
            console.log(this);
            let aa = {
					aaa:'xx',
					af:()=>{console.log(this)}
 				}
			aa.af()
        }
    };
    //<h1 id='d1'>xxx</h1>
    //document.getElementById('d1').onclick = function(){this.innerText;};
    person1.f1();  //通过自定对象来使用函数

    //ES6中自定义对象中来封装箭头函数的写法
    let username = '子俊'; //-- window.username
    let person2 = {
        name:'超',
        age:18,
        //f1:function(){};
        f1: () => {  //在自定义的对象中放函数的方法
            console.log(this); //this指向的不再是当前的对象了，而是指向了person的父级对象(称为上下文)，而此时的父级对象是我们的window对象，Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
            console.log(window);//还记得window对象吗，全局浏览器对象，打印结果和上面一样：Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
            console.log(this.username)  //'子俊'
        }
    };
    person2.f1(); //通过自定对象来使用函数
    person2 -- window.person2
    //而我们使用this的时候，希望this是person对象，而不是window对象，所以还有下面这种写法
    let person3 = {
        name:'超',
        age:18,
        //f1:function(){};
        //f1(){}
        f1(){  //相当于f1:function(){},只是一种简写方式,称为对象的单体模式写法，写起来也简单，vue里面会看用到这种方法
            console.log(this);//this指向的是当前的对象,{name: "超", age: 18, f1: ƒ}
            console.log(this.name)  //'超'
        }
    };
    person3.f1()




    let name2 = '昭志';
    let person2 = {
        name2:'虎凌',
        age:18,
        f1:() => {  
            console.log(this);
            console.log(this.name2)  
        }
    };
    person2.f1();
```



## es5和es6的类写法对比（了解）

```js
<script>
    //es5写类的方式
    
    function Person(name,age) {
        //封装属性
        this.name = name;  //this--Python的self
        this.age = age;
    }
    //封装方法，原型链
    Person.prototype.f1 = function () {
        console.log(this.name);//this指的是Person对象, 结果：'超'
    };
    //封装方法，箭头函数的形式写匿名函数
    Person.prototype.f2 = ()=>{
        console.log(this); //Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}  this指向的是window对象
    };

    var p1 = new Person('超',18);
    p1.age
    
    p1.f1();
    p1.f2();
    //其实在es5我们将js的基本语法的时候，没有将类的继承，但是也是可以继承的，还记得吗，那么你想，继承之后，我们是不是可以通过子类实例化的对象调用父类的方法啊，当然是可以的，知道一下就行了，我们下面来看看es6里面的类怎么写
    class Person2{
        constructor(name,age){ //对象里面的单体模式，记得上面将函数的时候的单体模式吗，这个方法类似于python的__init__()构造方法，写参数的时候也可以写关键字参数 constructor(name='超2',age=18)
            //封装属性
            this.name = name;
            this.age = age;
        }  //注意这里不能写逗号
        
        showname(){  //封装方法  
            console.log(this.name);
        }  //不能写逗号
        showage(){
            console.log(this.age);
        }
    }
    let p2 = new Person2('超2',18);
    p2.showname()  //调用方法  '超2'
    //es6的类也是可以继承的，这里咱们就不做细讲了，将来你需要的时候，就去学一下吧，哈哈，我记得是用的extends和super


</script>

```





# 1. vue.js的快速入门使用

## 1.1 vue.js库的下载

vue.js是目前前端web开发最流行的工具库，由尤雨溪在2014年2月发布的。

另外几个常见的工具库：react.js /angular.js



官方网站：

​	中文：https://cn.vuejs.org/

​	英文：https://vuejs.org/

官方文档：https://cn.vuejs.org/v2/guide/

vue.js目前有1.x、2.x和3.x 版本，我们学习2.x版本的。2.x到3.x是平滑过渡的，也就是说你之前用2.x写的代码，用3.x的版本的vue.js也是没问题的。



## 1.2 vue.js库的基本使用

在github下载：https://github.com/vuejs/vue/releases

在官网下载地址： <https://cn.vuejs.org/v2/guide/installation.html>

vue的引入类似于jQuery，开发中可以使用开发版本vue.js，产品上线要换成vue.min.js。



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="js/vue.min.js"></script>
    
</head>
<body>
<div id="app">
    <!-- {{ message }} 表示把vue对象里面data属性中的对应数据输出到页面中 -->
    <!-- 在双标签中显示数据要通过{{  }}来完成 -->
    <p>{{ message }}</p>
</div>
</body>
  <script>
    
      	// vue.js的代码开始于一个Vue对象。所以每次操作数据都要声明Vue对象开始。
        let vm = new Vue({
            el:'#app',   // 设置当前vue对象要控制的标签范围。
          	// data属性写法方式1
            data:{  // data是将要展示到HTML标签元素中的数据。
              message: 'hello world!',
            }
          	// 方式2
            // data:function () {
            //     return {
            //         'msg':'掀起你的盖头来1！'
            //     }
            // }
						// 方式3
            data(){   // 单体模式  这种写法用的居多，并且后面学习中有个地方一定要这样写，所以我们就记下来这种写法就可以了
                  return {
                      'msg':'掀起你的盖头来2！',
                  }
              }
            });
    
    </script>
</html>
```



![1607307150438](https://ssz29.oss-cn-chengdu.aliyuncs.com/img_for_md/1607307150438.png)



总结：

```html
1. vue的使用要从创建Vue对象开始
   var vm = new Vue();
   
2. 创建vue对象的时候，需要传递参数，是自定义对象，自定义对象对象必须至少有两个属性成员
   var vm = new Vue({
     el:"#app",  # 圈地:要通过vue语法来控制id属性值为app的标签, 在这个标签内部,就可以使用vue的语法,这个标签外部不能使用vue语法

	#数据属性定义的: 方式1
	 data: {
         数据变量:"变量值",
         数据变量:"变量值",
         数据变量:"变量值",
     },
	#方式2 (常用)
	data(){
            return {
                msg:'hello world',
            }
        }
   });
   
   el:圈地，划地盘，设置vue可以操作的html内容范围，值就是css的id选择器,其他选择器也可以，但是多用id选择器。
   data: 保存vue.js中要显示到html页面的数据。
   
3. vue.js要控制器的内容外围，必须先通过id来设置。
  <div id="app">
      <h1>{{message}}</h1>
      <p>{{message}}</p>
  </div>
```



vue中的变量可以直接进行一些简单直接的js操作

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>test vue</title>
</head>
<body>

<div id="app">
    <!-- vue的模板语法，和django的模板语法类似 -->
    <h2>{{ msg }}</h2> <!-- 放一个变量，会到data属性中去找对应的值 -->
    <!-- 有人说，我们直接这样写数据不就行吗，但是你注意，我们将来的数据都是从后端动态取出来的，不能写死这些数据啊，你说对不对 -->
    <h2>{{ 'hello beautiful girl!' }}</h2>  <!-- 直接放一个字符串 -->
    <h2>{{ num+1 }}</h2>  <!-- 四则运算 -->
  	<h2>{{ 2+1 }}</h2>  <!-- 四则运算 -->
    <h2>{{ {'name':'chao'} }}</h2> <!-- 直接放一个自定义对象 -->
    <h2>{{ person.name }}</h2>  <!-- 下面data属性里面的person属性中的name属性的值 -->
    <h2>{{ 1>2?'真的':'假的' }}</h2>  <!-- js的三元运算 -->
    <h2>{{ msg2.split('').reverse().join('') }}</h2>  <!-- 字符串反转 -->


</div>

<!-- 1.引包 -->
<script src="vue.js"></script>
<script>
//2.实例化对象
    new Vue({
        el:'#app',
        data:{
            msg:'黄瓜',
            person:{
                name:'超',
            },
            msg2:'hello Vue',
            num:10,
        }
    })

</script>
</body>
</html>
```





## 1.3 vue.js的M-V-VM思想

MVVM 是Model-View-ViewModel 的缩写，它是一种基于前端开发的架构模式。

`Model` 指代的就是vue对象的data属性里面的数据。这里的数据要显示到页面中。

`View`  指代的就是vue中数据要显示的HTML页面，在vue中，也称之为“视图模板” 。

`ViewModel ` 指代的是vue.js中我们编写代码时的vm对象了，它是vue.js的核心，负责连接 View 和 Model，保证视图和数据的一致性，所以前面代码中，data里面的数据被显示中p标签中就是vm对象自动完成的。

![1607307652917](https://ssz29.oss-cn-chengdu.aliyuncs.com/img_for_md/1607307652917.png)

编写代码，让我们更加清晰的了解MVVM：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.min.js"></script>
    <script>
    window.onload = function(){
        // 创建vm对象
        var vm = new Vue({
            el: "#app",
            data: {
                name:"大标题",
                age:16,
            },
        })
    }
    </script>
</head>
<body>
    <div id="app">
        <!-- 在双标签中显示数据要通过{{  }}来完成 -->
        <h1>{{name}}</h1>
        <p>{{age}}</p>
        <!-- 在表单输入框中显示数据要使用v-model来完成，模板语法的时候，我们会详细学习 -->
        <input type="text" v-model="name">
    </div>
</body>
</html>
```



在浏览器中可以在 console.log通过 vm对象可以直接访问el和data属性,甚至可以访问data里面的数据

```javascript
console.log(vm.$el)     # #box  vm对象可以控制的范围
console.log(vm.$data);  # vm对象要显示到页面中的数据
console.log(vm.message);# 这个 message就是data里面声明的数据,也可以使用 vm.变量名显示其他数据,message只是举例.
```







# 2.  Vue指令系统的常用指令

指令 (Directives) 是带有“v-”前缀的特殊属性。每一个指令在vue中都有固定的作用。

在vue中，提供了很多指令，常用的有：v-html、v-if、v-model、v-for等等。





## 2.1 文本指令v-html和v-text

​	v-text相当于js代码的innerText，相当于我们上面说的模板语法，直接在html中插值了，插的就是文本，如果data里面写了个标签，那么通过模板语法渲染的是文本内容，这个大家不陌生，这个v-text就是辅助我们使用模板语法的

​	**v-html相当于innerHtml**

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>

<div id="app" >
<!--    <h1>{{atag}}</h1> 和v-text指令系统做的是一样的-->
    <h1 v-text="atag"></h1>
    
    <h1 v-html="atag"></h1> <!-- 能过将标签字符串识别成标签效果 -->
<!--    <h1 v-text="num"></h1>-->
<!--    <h1 v-text="num+1"></h1>-->

</div>


</body>

<script src="vue.js"></script>
<script>

    let vm = new Vue({
        el:'#app',
        data(){
            return {
                atag:'<a href="http://www.baidu.com">百度</a>',
                num:20,
            }
        }


    })


</script>

</html>
```





## 2.2 条件渲染指令v-if和v-show

vue中提供了两个指令可以用于判断是否要显示元素，分别是v-if和v-show。

### 2.4.1 v-if

```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>

<div id="app" >
    <a href="" v-if="num>20">百度1</a>
    <a href="" v-else-if="num<20">百度2</a>
    <h1 v-else>京东</h1>

</div>


</body>

<script src="vue.js"></script>
<script>

    let vm = new Vue({
        el:'#app',
        data(){
            return {

                num:21,

            }
        }
        // vm.num = 18;  我们发现当num的值被重新赋值时,vue的视图部分(html)
        // 会自动发生变化,这就是数据驱动视图,以后视图的效果都是通过数据来控制的
        // vm.num = 20;

    })


</script>

</html>
```





### 2.2.4 v-show

```vue
标签元素：
			<h1 v-show="ok">Hello!</h1>
  data数据：
  		data:{
      		ok:false    // true则是显示，false是隐藏
      }
```





简单总结v-if和v-show

```

v-show后面不能v-else或者v-else-if

v-show隐藏元素时，使用的是display:none来隐藏的，而v-if是直接从HTML文档中移除元素[DOM操作中的remove]


v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。
```





## 2.3 操作属性v-bind

格式：

```
<标签名 :标签属性="data属性"></标签名>
```



```html
<p :title="str1">{{ str1 }}</p> <!-- 也可以使用v-html显示双标签的内容，{{  }} 是简写 -->
<a :href="url2">淘宝</a>
<a v-bind:href="url1">百度</a>  <!-- v-bind是vue1.x版本的写法 -->

简单示例
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>

<div id="app" >

    <h1 v-bind:xx="xxattr">xxoo</h1>  <!-- v-bind:动态的标签属性控制,简写 : -->
    <h1 :xx="xxattr">xxoo</h1>  <!-- v-bind:动态的标签属性控制,简写 : -->
</div>


</body>

<script src="vue.js"></script>
<script>

    let vm = new Vue({
        el:'#app',
        data(){
            return {

                xxattr:'sss',

            }
        }

    })

</script>

</html>
```



显示wifi密码效果：配合v-on事件绑定

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>

<div id="xx">
    <input :type="tt"> <button v-on:click="showp">{{msg}}</button>
    <input :type="tt"> <button @click="showp">{{msg}}</button>  <!-- v-on:事件名称简写@事件名称 -->

</div>
</body>

<script src="vue.js"></script>
<script>
    new Vue({
        el:'#xx',  // #xx   css选择器
        data(){
            return {
                tt:'password',
                msg:'显示密码',
            }
        },
        methods:{
            // showp:function (){
            //
            // }
            showp(){ // 单体模式

                if (this.tt === 'password'){
                    this.tt = 'text';
                    this.msg = '隐藏密码';
                }else {
                    this.tt = 'password';
                    this.msg = '显示密码';
                }

            }
        }

    })

</script>
</html>
```



## 2.4 事件绑定v-on和methods属性

有两种事件操作的写法，@事件名 和 v-on:事件名

```html
<button v-on:click="num++">按钮</button>   <!-- v-on 是vue1.x版本的写法 -->
<button @click="num+=5">按钮2</button>
```

v-on控制数据属性的方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
<div id="xx">
<!--    <button @click="num++">+1</button>  直接操作数据属性 -->
    <button @click="num=num+10">+10</button>  <!-- 直接操作数据属性 -->
    <h1>{{num}}</h1>
</div>

</body>
<script src="vue.js"></script>
<script>
    new Vue({
        el:'#xx',  // #xx   css选择器
        data(){
            return {
                num:100,
            }
        },
        methods:{
        }
    })

</script>

</html>
```





总结：

```
1. 使用@事件名来进行事件的绑定
   语法：
      <h1 @click="num++">{{num}}</h1>

2. 绑定的事件的事件名，全部都是js的事件名：
   @submit   --->  onsubmit
   @focus    --->  onfocus
   ....

```



#### 例2:完成商城的商品增加减少数量

步骤：

1. 给vue对象添加操作数据的方法
2. 在标签中使用指令调用操作数据的方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
<div id="xx">
<button @click="num++">+1</button>
    <input type="text" :value="num">
<button @click="jian">-1</button> <!-- 一些复杂的数据处理还是通过方法来搞 -->
</div>

</body>
<script src="vue.js"></script>
<script>
    new Vue({
        el:'#xx',  // #xx   css选择器
        data(){
            return {
                num:10,
            }
        },
        methods:{
            jian(){
                if (this.num>0){
                    this.num--;
                }

            }
        }

    })


</script>

</html>
```



## v-model

双向数据绑定

![1607313007270](https://ssz29.oss-cn-chengdu.aliyuncs.com/img_for_md/1607313007270.png)



代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
<div id="xx">
    <input type="text" v-model="num">

    <h1>{{num}}</h1>

</div>

</body>
<script src="vue.js"></script>
<script>
    new Vue({
        el:'#xx',  // #xx   css选择器
        data(){
            return {
                num:10,
            }
        },
        methods:{
            jian(){
                if (this.num>0){
                    this.num--;
                }

            }
        }

    })


</script>

</html>
```



## 2.5 操作样式

### 2.5.1 控制标签class类名

```
格式：
   <h1 :class="值">元素</h1>  值可以是对象、对象名、数组（数组的方式用的比较少）
   
   data(){
            return {
                num:11,
                xx:'c1',
            }
        }
```

示例:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        .c1{
            color:red;
        }
        .c2{
            color:green;
        }
    </style>

</head>
<body>
<div id="xx">

    <p :class="xx">床前明月光</p>


</div>

</body>
<script src="vue.js"></script>
<script>
    new Vue({
        el:'#xx',  // #xx   css选择器
        data(){
            return {
                num:11,
                xx:'c1',
            }
        },
        methods:{
            jian(){
                if (this.num>0){
                    this.num--;
                }

            }
        }

    })


</script>

</html>
```



示例2: 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        .c1{
            color:red;
        }
        .c2{
            color:green;
        }
    </style>

</head>
<body>
<div id="xx">
<!--    <p class="c1" :class="{c2:num<=10}">床前明月光</p>-->
<!--    <p :class="{c1:num>10,c2:num<=10}">床前明月光</p>-->
    <!-- class类值控制语法: :class='{类值:判断条件(布尔值或者得到布尔值的算式),类值:判断条件....}'
        布尔值或者得到布尔值的算式: 里面直接可以使用数据属性
    -->

</div>

</body>
<script src="vue.js"></script>
<script>
    new Vue({
        el:'#xx',  // #xx   css选择器
        data(){
            return {
                num:11,
                xx:'c1',
            }
        },

    })

</script>

</html>
```



### 2.5.2 控制标签style样式

```html
格式1：值是json对象，对象写在元素的:style属性中
	 标签元素：
		     <div :style="{color: activeColor, fontSize: fontSize + 'px' }"></div>
				<!-- 注意：不能出现中横杠，有的话就套上'font-size'，或者去掉横杠，后一个单词的首字母大写，比如fontSize -->
	 data数据如下：
         data: {
             activeColor: 'red',
             fontSize: 30
         }

示例:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">

</head>
<body>
<div id="xx">
    <div class="c1" :style="{color:c, backgroundColor:b}"> <!-- 注意:有-的css属性名称,要改为驼峰格式 -->
        床前明月光,地上鞋三双.

    </div>
</div>

</body>
<script src="vue.js"></script>
<script>
    new Vue({
        el:'#xx',  // #xx   css选择器
        data(){
            return {
                c:'red',
                b:'green'
            }
        },
        methods:{
            jian(){
                if (this.num>0){
                    this.num--;
                }

            }
        }

    })


</script>

</html>



格式2：值是对象变量名，对象在data中进行声明
   标签元素：
   			<div v-bind:style="styleObject"></div>
   data数据如下：
         data: {
            	styleObject: {
             		color: 'red',
             		fontSize: '13px'  
				   }
				 }

示例:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">

</head>
<body>
<div id="xx">

    <div class="c1" :style="divstyle"> <!-- 注意:有-的css属性名称,要改为驼峰格式 -->
        床前明月光,地上鞋三双.

    </div>
</div>

</body>
<script src="vue.js"></script>
<script>
    new Vue({
        el:'#xx',  // #xx   css选择器
        data(){
            return {
                c:'red',
                b:'green',
                divstyle:{
                    'color':'tan',
                    'backgroundColor':'blue',

                }
            }
        },
        methods:{
            jian(){
                if (this.num>0){
                    this.num--;
                }

            }
        }

    })


</script>

</html>


```





### 2.5.2 实例-vue版本选项卡

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        #card{
            width: 500px;
            height: 350px;
        }
        .title{
            height:50px;
        }
        .title span{
            width: 100px;
            height: 50px;
            background-color:#ccc;
            display: inline-block;
            line-height: 50px; /* 设置行和当前元素的高度相等,就可以让文本内容上下居中 */
            text-align:center;
        }
        .content .list{
            width: 500px;
            height: 300px;
            background-color: yellow;
            display: none;
        }
        .content .active{
            display: block;
        }

        .title .current{
            background-color: yellow;
        }
    </style>
    <script src="vue.js"></script>
</head>
<body>

    <div id="card">
        <div class="title">
            <span @click="num=1" :class="{current:num===1}">国内新闻</span>
            <span @click="num=2" :class="{current:num===2}">国际新闻</span>
            <span @click="num=3" :class="{current:num===3}">银河新闻</span>
            <!--<span>{{num}}</span>-->
        </div>
        <div class="content">
            <div class="list" :class="{active:num===1}">国内新闻列表</div>
            <div class="list" :class="{active:num===2}">国际新闻列表</div>
            <div class="list" :class="{active:num===3}">银河新闻列表</div>
        </div>
    </div>
    <script>
        // 思路：
        // 当用户点击标题栏的按钮[span]时，显示对应索引下标的内容块[.list]
        // 代码实现：
        var card = new Vue({
            el:"#card",
            data:{
                num:0,
            },
        });
    </script>

</body>
</html>
```





## 2.6 列表渲染指令v-for

在vue中，可以通过v-for指令可以将一组数据渲染到页面中，数据可以是数组或者对象。

```html
数据是数组：        
 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <script src="vue.js"></script>
</head>
<body>

    <div id="app">
        <ul>
            <li v-for="(value,index) in hobby_list" :key="index">{{value}}--{{index}}</li>
            <!-- value是数组中的元素,index是索引,注意v-for一定要写 :key -->
<!--            <li v-for="value, index in hobby_list">{{value}}</li>-->
        </ul>

    </div>



</body>
<script>
        // 思路：
        // 当用户点击标题栏的按钮[span]时，显示对应索引下标的内容块[.list]
        // 代码实现：
        var card = new Vue({
            el:"#app",
            data:{
                num:0,
                hobby_list:[
                    '抽烟',
                    '喝酒',
                    '烫头',
                    '烫头2',
                ]
            },
        });
    </script>
</html>


数据是对象：
        <ul>
            <!--i是每一个value值-->
            <li v-for="value in book">{{value}}</li>
        </ul>
        <ul>
            <!--value是每一个value值,attr是每一个键名-->
            <li v-for="(value,attr) in book">{{attr}}:{{value}}</li>
        </ul>
        <script>
            var vm1 = new Vue({
                el:"#app",
                data:{
                    book: {
                        // "attr属性名":"value属性值"
                        "id":11,
                        "title":"图书名称1",
                        "price":200
                    },
                },
            })
        </script>

```

练习：

```html
goods:[
	{"name":"python入门","price":150},
	{"name":"python进阶","price":100},
	{"name":"python高级","price":75},
	{"name":"python研究","price":60},
	{"name":"python放弃","price":110},
]

# 把上面的数据采用table表格输出到页面，价格大于60的那一条数据需要添加背景色

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .bb{
            background-color: tan;
        }

    </style>
    <script src="vue.js"></script>
</head>
<body>

    <div id="app">
        <ul>
            <li v-for="(value,index) in hobby_list" :key="index">{{value}}--{{index}}</li>
            <!-- value是数组中的元素,index是索引,注意v-for一定要写 :key -->
<!--            <li v-for="value, index in hobby_list">{{value}}</li>-->
        </ul>

        <ul>
            <li v-for="(value,index) in person_info" :key="index">{{value}} -- {{index}}</li>
        </ul>

        <table border="1">
            <thead>
            <tr>
                <th>商品名称</th>
                <th>价格</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(value,index) in goods" :key="index" :class="{bb:value.price>60}">
                <td>{{value.name}}</td>
                <td>{{value.price}}</td>
            </tr>
            </tbody>

        </table>

    </div>



</body>
<script>
        // 思路：
        // 当用户点击标题栏的按钮[span]时，显示对应索引下标的内容块[.list]
        // 代码实现：
        var card = new Vue({
            el:"#app",
            data:{
                num:0,
                hobby_list:[
                    '抽烟',
                    '喝酒',
                    '烫头',
                    '烫头2',
                ],
                person_info:{
                    name:'永飞',
                    age:99,
                    sex:'unknown',
                },
                goods:[
                    {"name":"python入门","price":150},
                    {"name":"python进阶","price":100},
                    {"name":"python高级","price":75},
                    {"name":"python研究","price":60},
                    {"name":"python放弃","price":110},
                ]
            },
        });
    </script>
</html>
```







# 3. Vue对象提供的属性功能

## 3.1 过滤器

过滤器，就是vue允许开发者自定义的文本格式化函数，可以使用在两个地方：输出内容和操作数据中。

定义过滤器的方式有两种,全局和局部过滤器  

### 3.1.1 使用Vue.filter()进行全局定义

```javascript
Vue.filter("RMB1", function(v){
  	//就是来格式化(处理)v这个数据的
  	if(v==0){
    		return v
  	}

  	return v+"元"
})
```



### 3.1.2 在vue对象中通过filters属性来定义

```javascript
var vm = new Vue({
  el:"#app",
  data:{},
  filters:{
    RMB2:function(value){
      if(value==''){
        return;
      }else{
      	return '¥ '+value;
      }
    }
	}
});
```



示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .bb{
            background-color: tan;
        }

    </style>
    <script src="vue.js"></script>
</head>
<body>

    <div id="app">
        <h1>{{price|yuan}}</h1>
<!--        <h1>{{price.toFixed(3)}}</h1>-->
        <h1>{{price|keeptwopoint(1)}}</h1>
        <h1>{{price|keeptwopoint(1,2,3)}}</h1>  <!-- 传递多个参数 -->
        <h1>{{price|RMB}}</h1>
        <h1>{{price|keeptwopoint(2)|RMB}}</h1> <!-- 连续使用多个过滤器 -->

    </div>



</body>
<script>

        // 全局过滤器,任何对象都可以直接使用这个过滤器
        Vue.filter('RMB',function (val){

            return val + 'RMB'
        })

        var card = new Vue({
            el:"#app",
            data:{
                price:100,

            },
            methods:{

            },
            // 局部过滤器,只能在当前对象中使用
            filters:{
                yuan(val){

                    return val + '元'
                },
                keeptwopoint(val,n){

                    return val.toFixed(n)
                }
            }


        });
    </script>
</html>
```





## 3.2 计算和侦听属性

### 3.2.1 计算属性

我们之前学习过字符串反转，如果直接把反转的代码写在元素中，则会使得其他同事在开发时时不易发现数据被调整了，所以vue提供了一个计算属性(computed)，可以让我们把调整data数据的代码存在在该属性中。其实计算属性主要用于监听，可以监听多个对象，后面学了监听之后再说。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .bb{
            background-color: tan;
        }

    </style>
    <script src="vue.js"></script>
</head>
<body>

    <div id="app">

        <h1>{{showpp}}</h1>
    </div>



</body>
<script>

        var card = new Vue({
            el:"#app",
            data:{
                price:100,
                name:'昭志',
                pp:'的pp'

            },
            methods:{

            },
            computed:{
                showpp(){

                    let s = this.name + ' ' + this.pp;
                    return s
                },
                


            }


        });
    </script>
</html>
```





### 3.2.2 监听属性

侦听属性，可以帮助我们侦听data某个数据的变化，从而做相应的自定义操作。

侦听属性是一个对象，它的键是要监听的对象或者变量，值一般是函数，当侦听的data数据发生变化时，会自定执行的对应函数，这个函数在被调用时，vue会传入两个形参，第一个是变化后的数据值，第二个是变化前的数据值。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .bb{
            background-color: tan;
        }

    </style>
    <script src="vue.js"></script>
</head>
<body>

    <div id="app">

        <button @click="price+=1">价钱!!</button>
        <h1>{{price}}</h1>
        <input type="text" v-model="info.num">
    </div>



</body>
<script>



        var card = new Vue({
            el:"#app",
            data:{
                price:100,
                name:'昭志',
                pp:'的pp',
                info:{
                    num:20
                }

            },
            methods:{

            },
            watch:{

                // 'pp':function (){
                //
                // },

                // price(){
                //     alert(this.name + this.pp + '有危险!!!!')
                // }

                //  不支持这种写法
                // info.num(){
                //     console.log(this.info.num);
                // }
                // 监听嵌套数据的写法
                'info.num':function (){
                    console.log(this.info.num);
                }
            }


        });
    </script>
</html>
```



示例：用户名长度限制不能长于6位

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .bb{
            background-color: tan;
        }

    </style>
    <script src="vue.js"></script>
</head>
<body>

    <div id="app">

        <input type="text" v-model="username">
    </div>



</body>
<script>
    
        var card = new Vue({
            el:"#app",
            data:{
                username:''

            },
            methods:{

            },
            watch:{
                username(){
                    console.log(this.username.length);
                    if (this.username.length > 6){
                        alert('差不多得了');
                        this.username = this.username.slice(0,6);
                    }
                }

            }


        });
    </script>
</html>
```





## 3.3 vue对象的生命周期钩子函数

每个Vue对象在创建时都要经过一系列的初始化过程。在这个过程中Vue.js会自动运行一些叫做生命周期的的钩子函数，我们可以使用这些函数，在对象创建的不同阶段加上我们需要的代码，实现特定的功能。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.min.js"></script>
    <script>
    window.onload = function(){
        //$(function(){})()  $.ready() -- window.onload = function(){}
        var vm = new Vue({
            el:"#app",
            data:{
                num:0
            },
            beforeCreate:function(){
                console.log("beforeCreate,vm对象尚未创建,num="+ this.num);  //undefined，就是说data属性中的值还没有放到vm对象中
                this.name=10; // 此时没有this对象呢，所以设置的name无效，被在创建对象的时候被覆盖为0
              	console.log(this.$el) //undefined
            },
            created:function(){
              	// 用的居多，一般在这里使用ajax去后端获取数据，然后交给data属性
                console.log("created,vm对象创建完成,设置好了要控制的元素范围,num="+this.num );  // 0 也就是说data属性中的值已经放到vm对象中
                this.num = 20;
                console.log(this.$el) //undefined
            },
            beforeMount:function(){
                console.log( this.$el.innerHTML ); // <p>{{num}}</p> ,vm对象已经帮我们获取到了这个视图的id对象了
                console.log("beforeMount,vm对象尚未把data数据显示到页面中,num="+this.num ); // 20,也就是说vm对象还没有将数据添加到我们的视图中的时候
                this.num = 30;
            },
            mounted:function(){
              	// 用的居多，一般在这里使用ajax去后端获取数据然后通过js代码对页面中原来的内容进行更改 
                console.log( this.$el.innerHTML ); // <p>30</p>
                console.log("mounted,vm对象已经把data数据显示到页面中,num="+this.num); // 30,也就是说vm对象已经将数据添加到我们的视图中的时候
            },
            
          	// 后面两个简单作为了解吧，测试的时候最好单独测试下面两个方法
            beforeUpdate:function(){
                // this.$el 就是我们上面的el属性了，$el表示当前vue.js所控制的元素#app
                console.log( this.$el.innerHTML );  // <p>30</p>
                console.log("beforeUpdate,vm对象尚未把更新后的data数据显示到页面中,num="+this.num); // beforeUpdate----31
                
            },
            updated:function(){
                console.log( this.$el.innerHTML ); // <p>31</p>
                console.log("updated,vm对象已经把过呢更新后的data数据显示到页面中,num=" + this.num ); // updated----31
            },
        });
    }
    </script>
</head>
<body>
    <div id="app">
        <p>{{num}}</p>
        <button @click="num++">按钮</button>
    </div>
</body>
</html>
```



总结：

```
在vue使用的过程中，如果要初始化操作，把初始化操作的代码放在 mounted 中执行。
mounted阶段就是在vm对象已经把data数据实现到页面以后。一般页面初始化使用。例如，用户访问页面加载成功以后，就要执行的ajax请求。

另一个就是created，这个阶段就是在 vue对象创建以后，把ajax请求后端数据的代码放进 created
```





## 3.4 阻止事件冒泡

使用.stop和.prevent

示例1：阻止事件冒泡

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .c1{
            background-color: tan;
            height: 200px;
        }
        .c2{
            background-color: red;
            height: 100px;
            width: 100px;
        }

    </style>
    <script src="vue.js"></script>
</head>
<body>

    <div id="app">

        <div class="c1" @click="f1">
            <!-- 阻止事件冒泡 -->
<!--            <div class="c2" @click.stop="f2"></div>-->
            <div class="c2" @click.stop.prevent="f2"></div>
        </div>

        <!-- 阻止标签默认动作 -->
<!--        <a href="http://www.baidu.com" @click.stop.prevent="">百度</a>-->

    </div>



</body>
<script>

        var card = new Vue({
            el:"#app",
            data:{
                username:'chao'

            },
            methods:{
                f1(){
                    alert('负极标签');
                },
                f2(){

                    alert('子标签')

                }

            },



        });


    </script>
</html>
```





## 3.5综合案例-todolist

我的计划列表

html代码:

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>todolist</title>
	<style type="text/css">
		.list_con{
			width:600px;
			margin:50px auto 0;
		}
		.inputtxt{
			width:550px;
			height:30px;
			border:1px solid #ccc;
			padding:0px;
			text-indent:10px;
		}
		.inputbtn{
			width:40px;
			height:32px;
			padding:0px;
			border:1px solid #ccc;
		}
		.list{
			margin:0;
			padding:0;
			list-style:none;
			margin-top:20px;
		}
		.list li{
			height:40px;
			line-height:40px;
			border-bottom:1px solid #ccc;
		}

		.list li span{
			float:left;
		}

		.list li a{
			float:right;
			text-decoration:none;
			margin:0 10px;
		}
	</style>
</head>
<body>
	<div class="list_con">
		<h2>To do list</h2>
		<input type="text" name="" id="txt1" class="inputtxt">
		<input type="button" name="" value="增加" id="btn1" class="inputbtn">

		<ul id="list" class="list">
			<!-- javascript:; # 阻止a标签跳转 -->
			<li>
				<span>学习html</span>
				<a href="javascript:;" class="up"> ↑ </a>
				<a href="javascript:;" class="down"> ↓ </a>
				<a href="javascript:;" class="del">删除</a>
			</li>
			<li><span>学习css</span><a href="javascript:;" class="up"> ↑ </a><a href="javascript:;" class="down"> ↓ </a><a href="javascript:;" class="del">删除</a></li>
			<li><span>学习javascript</span><a href="javascript:;" class="up"> ↑ </a><a href="javascript:;" class="down"> ↓ </a><a href="javascript:;" class="del">删除</a></li>
		</ul>
	</div>
</body>
</html>
```



特效实现效果：

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>todolist</title>
	<style type="text/css">
		.list_con{
			width:600px;
			margin:50px auto 0;
		}
		.inputtxt{
			width:550px;
			height:30px;
			border:1px solid #ccc;
			padding:0px;
			text-indent:10px;
		}
		.inputbtn{
			width:40px;
			height:32px;
			padding:0px;
			border:1px solid #ccc;
		}
		.list{
			margin:0;
			padding:0;
			list-style:none;
			margin-top:20px;
		}
		.list li{
			height:40px;
			line-height:40px;
			border-bottom:1px solid #ccc;
		}

		.list li span{
			float:left;
		}

		.list li a{
			float:right;
			text-decoration:none;
			margin:0 10px;
		}
	</style>
    <script src="js/vue.js"></script>
</head>
<body>
	<div id="todolist" class="list_con">
		<h2>To do list</h2>
		<input type="text" v-model="message" class="inputtxt">
		<input type="button" @click="addItem" value="增加" class="inputbtn">
		<ul id="list" class="list">
			<li v-for="item,key in dolist">
				<span>{{item}}</span>
				<a @click="upItem(key)" class="up" > ↑ </a>
				<a @click="downItem(key)" class="down"> ↓ </a>
				<a @click="delItem(key)" class="del">删除</a>
			</li>
		</ul>
	</div>
    <script>
    // 计划列表代码
    let vm = new Vue({
        el:"#todolist",
        data:{
            message:"",
            dolist:[
                "学习html",
                "学习css",
                "学习javascript",
            ]
        },
        methods:{
            addItem(){
                if(this.messsage==""){
                    return false;
                }

                this.dolist.push(this.message);
                this.message = ""
            },
            delItem(key){
                this.dolist.splice(key, 1);
            },
            upItem(key){
                if(key==0){
                    return false;
                }
                // 向上移动
                let result = this.dolist.splice(key,1); // 注意返回的是数组
                this.dolist.splice(key-1,0,result[0]);
            },
            downItem(key){
                // 向下移动
                let result = this.dolist.splice(key, 1);
                console.log(result); 
                this.dolist.splice(key+1,0,result[0]);
            }
        }
    })
    </script>
</body>
</html>
```





splice方法的简单使用

```js
删除
let xx = [11,22,33,44,55];
xx.splice(2,2);

删除替换
let xx = [11,22,33,44,55];
xx.splice(2,2,'aa','bb','cc','dd');
[11, 22, "aa", "bb", "cc", "dd", 55]
```



a标签href属性

```html

<a href="#">百度</a>
<!--href属性为空时，刷新页面-->
<!--不写href属性，就是普通文本标签-->
<!-- 当href属性等于某个值时，页面跳转到对应的页面 -->
<!-- 当href属性等于javascript:void (0);简写javascript:;时，不刷新也不跳转 -->
<!-- 当href属性等于#时，不刷新也不跳转，但是会在url上加个#号结尾 -->
```

# 4. 通过axios实现数据请求

vue.js默认没有提供ajax功能的。

所以使用vue的时候，一般都会使用axios的插件来实现ajax与后端服务器的数据交互。

注意，axios本质上就是javascript的ajax封装，所以会被同源策略限制。

下载地址：

```
https://unpkg.com/axios@0.18.0/dist/axios.js
https://unpkg.com/axios@0.18.0/dist/axios.min.js

```

axios提供发送请求的常用方法有两个：axios.get() 和 axios.post() 。

增   post

删   delete

改   put

查   get  

```javascript
	// 发送get请求
    // 参数1: 必填，字符串，请求的数据接口的url地址，例如请求地址：http://www.baidu.com?id=200
    // 参数2：可选，json对象，要提供给数据接口的参数
    // 参数3：可选，json对象，请求头信息
	axios.get('服务器的资源地址',{ // http://www.baidu.com
    	params:{
    		参数名:'参数值', // id: 200,
    	}
    
    }).then(function (response) { // 请求成功以后的回调函数
    		console.log("请求成功");
    		console.log(response);
    
    }).catch(function (error) {   // 请求失败以后的回调函数
    		console.log("请求失败");
    		console.log(error.response);
    });

	// 发送post请求，参数和使用和axios.get()一样。
    // 参数1: 必填，字符串，请求的数据接口的url地址
    // 参数2：必填，json对象，要提供给数据接口的参数,如果没有参数，则必须使用{}
    // 参数3：可选，json对象，请求头信息
    axios.post('服务器的资源地址',{
    	username: 'xiaoming',
    	password: '123456'
    },{
        responseData:"json",
    })
    .then(function (response) { // 请求成功以后的回调函数
      console.log(response);
    })
    .catch(function (error) {   // 请求失败以后的回调函数
      console.log(error);
    });

	
	// b'firstName=Fred&lastName=Flintstone'
```







### 4.2.1 数据接口

数据接口，也叫api接口，表示`后端提供`操作数据/功能的url地址给客户端使用。

客户端通过发起请求向服务端提供的url地址申请操作数据【操作一般：增删查改】

同时在工作中，大部分数据接口都不是手写，而是通过函数库/框架来生成。





![1607396664663](assets/1607396664663.png)



### 4.2.3 ajax的使用

ajax的使用必须与服务端程序配合使用，但是目前我们先学习ajax的使用，所以暂时先不涉及到服务端python代码的编写。因此，我们可以使用别人写好的数据接口进行调用。

jQuery将ajax封装成了一个函数$.ajax()，我们可以直接用这个函数来执行ajax请求。

| 接口         | 地址                                                         |
| ------------ | ------------------------------------------------------------ |
| 天气接口     | http://wthrcdn.etouch.cn/weather_mini?city=城市名称          |
| 音乐接口搜索 | http://tingapi.ting.baidu.com/v1/restserver/ting?method=baidu.ting.search.catalogSug&query=歌曲标题 |
| 音乐信息接口 | http://tingapi.ting.baidu.com/v1/restserver/ting?method=baidu.ting.song.play&songid=音乐ID |



编写代码获取接口提供的数据：

jQ版本

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
    $(function(){
        $("#btn").on("click",function(){
            $.ajax({
                // 后端程序的url地址
                url: 'http://wthrcdn.etouch.cn/weather_mini',
                // 也可以使用method，提交数据的方式，默认是'GET'，常用的还有'POST'
                type: 'get', 
                dataType: 'json',  // 返回的数据格式，常用的有是'json','html',"jsonp"
                data:{ // 设置发送给服务器的数据，如果是get请求，也可以写在url地址的?后面
                    "city":'北京'
                }
                
            })
            .done(function(resp) {     // 请求成功以后的操作,新的ajax是这样写的，之前是通过属性success:function(){}的形式来写的
                console.log(resp);
            })
            .fail(function(error) {    // 请求失败以后的操作 error:function(){}
                console.log(error);
            });
        });
    })
    </script>
</head>
<body>
<button id="btn">点击获取数据</button>
</body>
</html>
```

vue版本：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.js"></script>
    <script src="js/axios.js"></script>
</head>
<body>
    <div id="app">
        <input type="text" v-model="city">
        <button @click="get_weather">点击获取天气</button>
    </div>
    <script>
        let vm = new Vue({
            el:"#app",
            data:{
                city:"",
            },
            methods:{
                get_weather(){
                    // http://wthrcdn.etouch.cn/weather_mini?city=城市名称
                    axios.get("http://wthrcdn.etouch.cn/weather_mini?city="+this.city)
                        .then(response=>{
                            console.log(response);

                        }).catch(error=>{
                            console.log(error.response)
                    });
                }
            }
        })
    </script>
</body>
</html>
```



总结：

```javascript
1. 发送ajax请求，要通过$.ajax()，参数是对象，里面有固定的参数名称。
   $.ajax({
     "url":"数据接口url地址",
     "method":"http请求方式，前端只支持get和post",
     "dataType":"设置服务器返回的数据格式，常用的json，html，jsonp，默认值就是json",
     // 要发送给后端的数据参数，post时，数据必须写在data，get可以写在data,也可以跟在地址栏?号后面
     "data":{
       "数据名称":"数据值",
     }
   }).then(function(resp){ // ajax请求数据成功时会自动调用then方法的匿名函数
     console.log( resp ); // 服务端返回的数据
   }).fail(function(error){ // ajax请求数据失败时会自动调用fail方法的匿名函数
     console.log( error );
   });
   
2. ajax的使用往往配合事件/钩子操作进行调用。
```







### 4.2.4 同源策略

同源策略，是浏览器为了保护用户信息安全的一种安全机制。所谓的同源就是指代通信的两个地址（例如服务端接口地址与浏览器客户端页面地址）之间比较，是否协议、域名(IP)和端口相同。不同源的客户端脚本[javascript]在没有明确授权的情况下，没有权限读写对方信息。



ajax本质上还是javascript，是运行在浏览器中的脚本语言，所以会被受到浏览器的同源策略所限制。

| 前端地址：`http://www.oldboy.cn/index.html` | 是否同源 | 原因                     |
| ------------------------------------------- | -------- | ------------------------ |
| `http://www.oldboy.cn/user/login.html`      | 是       | 协议、域名、端口相同     |
| `http://www.oldboy.cn/about.html`           | 是       | 协议、域名、端口相同     |
| `https://www.oldboy.cn/user/login.html`     | 否       | 协议不同 ( https和http ) |
| `http:/www.oldboy.cn:5000/user/login.html`  | 否       | 端口 不同( 5000和80)     |
| `http://bbs.oldboy.cn/user/login.html`      | 否       | 域名不同 ( bbs和www )    |

同源策略针对ajax的拦截，代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.js"></script>
    <script src="js/axios.js"></script>
</head>
<body>
    <div id="app">
        <button @click="get_music">点击获取天气</button>
    </div>
    <script>
        let vm = new Vue({
            el:"#app",
            data:{},
            methods:{
                get_music(){
                    axios.get("http://tingapi.ting.baidu.com/v1/restserver/ting?method=baidu.ting.search.catalogSug&query=我的中国心")
                        .then(response=>{
                            console.log(response);

                        }).catch(error=>{
                            console.log(error.response)
                    });
                }
            }
        })
    </script>
</body>
</html>
```



上面代码运行错误如下：

```
Access to XMLHttpRequest at 'http://tingapi.ting.baidu.com/v1/restserver/ting?method=baidu.ting.search.catalogSug&query=%E6%88%91%E7%9A%84%E4%B8%AD%E5%9B%BD%E5%BF%83' from origin 'http://localhost:63342' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

上面错误，关键词：Access-Control-Allow-Origin

只要出现这个关键词，就是访问受限。出现同源策略的拦截问题。







### 4.2.5 ajax跨域(跨源)方案之CORS

​      CORS是一个W3C标准，全称是"跨域资源共享"，它允许浏览器向跨源的后端服务器发出ajax请求，从而克服了AJAX只能同源使用的限制。

​      实现CORS主要依靠<mark>后端服务器中响应数据中设置响应头信息返回</mark>的。



django的视图

```python
def data(request):
	info = {'name': '世元', 'age': 18}
	ret = JsonResponse(info)
	# ret['Access-Control-Allow-Origin'] = 'http://127.0.0.1:8000'  #允许该地址的请求正常获取响应数据
    # ret['Access-Control-Allow-Origin'] = 'http://127.0.0.1:8000,http://127.0.0.1:8002'
	ret['Access-Control-Allow-Origin'] = '*' #允许所有地址请求都能拿到数据
    
	return ret
```


## 5.2 组件传值



通过prop属性进行传值

### 5.2.1 父组件往子组件传值 



两步：　　

​		1.父组件在使用子组件时要定义自定义的属性:比如:`<Vheader :ff="num"></Vheader>`动态传值

​			静态传值:`<Vheader ff="123"></Vheader>`

​		2.在子组件中使用props属性声明:比如:props:['ff', ]，然后子组件就可以将ff作为一个数据属性来使用了,比如:

```html
		template:`<div>
					<h1>{{nav_list}}</h1>
					<h2>{{nav_list}}</h2>
					<h4 style="color:green;">{{ff}}</h4>

				</div>`,
```



　　

看示例：

```vue
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>todolist</title>

</head>
<body>
	<div id="app">
		<!-- 3.使用子组件  用子 -->
<!--		<xx></xx>-->
<!--		<xx2></xx2>-->
	<App></App>
<!--	content  header footer aside section nav H5有一些新标签,注意,组件名称尽量不要和标签名称冲突 -->
	</div>
</body>
<script src="vue.js"></script>
<script src="axios.js"></script>
<script>

    // 父组件往子组件传值:



	let Vheader = {
		data(){
			return {
				nav_list:['个人中心','登录','注册','username']
			}
		},
		template:`<div>
					<h1>{{nav_list}}</h1>
					<h2>{{nav_list}}</h2>
					<h4 style="color:green;">{{ff}}</h4>

				</div>`,

		props:['ff', ],

	};


	let xxx = {
		data(){
			return {
				content_list:['xxx','ooo',]
			}
		},
		template:'<h1 style="color:red;">{{content_list}}</h1>',


	};

	let App = {
		data(){
			return {
				'xxx':'ooo',
				num:1000,
			}
		},
		// <Vheader ff="123"></Vheader> 静态传值
		template:
				`
				<div class="app">
					<Vheader :ff="num"></Vheader>
					<xxx></xxx>

				</div>

				`,
		components: {
			Vheader,
			xxx,
		}
	}


	let vm  = new Vue({
		el:'#app',

		components:{
			App,
		}


	})



</script>
</html>
```



使用父组件传递数据给子组件时, 注意一下几点:

1. 传递数据是变量,则需要在属性左边添加冒号.

   传递数据是变量,这种数据称之为"动态数据传递"

   传递数据不是变量,这种数据称之为"静态数据传递"

2. 父组件中修改了数据,在子组件中会被同步修改,但是,子组件中的数据修改了,是不是影响到父组件中的数据.

   这种情况,在开发时,也被称为"单向数据流"




```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>todolist</title>

</head>
<body>
	<div id="app">
		<!-- 3.使用子组件  用子 -->
<!--		<xx></xx>-->
<!--		<xx2></xx2>-->
	<App></App>
<!--	content  header footer aside section nav H5有一些新标签,注意,组件名称尽量不要和标签名称冲突 -->
	</div>
</body>
<script src="vue.js"></script>
<script src="axios.js"></script>
<script>

    // 父组件往子组件传值:



	let Vheader = {
		data(){
			return {
				nav_list:['个人中心','登录','注册','username']
			}
		},
		template:`<div>
					<h1>{{nav_list}}</h1>
					<h2>{{nav_list}}</h2>
					<h4 style="color:green;">{{ff}}</h4>
					<input type="text" v-model="ff">  #修改input标签的值,不会影响父级标签的数据

				</div>`,

		props:['ff', ],

	};


	let xxx = {
		data(){
			return {
				content_list:['xxx','ooo',]
			}
		},
		template:'<h1 style="color:red;">{{content_list}}</h1>',


	};

	let App = {
		data(){
			return {
				'xxx':'ooo',
				num:1001,
			}
		},
		// <Vheader ff="123"></Vheader> 静态传值
		template:
				`
				<div class="app">
					<Vheader :ff="num"></Vheader>
					<xxx></xxx>
					<h3 style="color:blue;">{{num}}</h3>

				</div>

				`,
		components: {
			Vheader,
			xxx,
		}
	}


	let vm  = new Vue({
		el:'#app',

		components:{
			App,
		}


	})



</script>
</html>
```





### 5.2.2 子组件父组件传值 

两步：

　　a.子组件中使用this.$emit('fatherHandler',val);fatherHandler是父组件中使用子组件的地方添加的绑定自定义事件，**注意，如果fatherHandler报错了，那么可能是你的vue版本不支持自定义键名称fatherHandler中有大写字母，所以我们改成father-handler或者直接就全部小写就可以了**

```vue
<Vheader @fatherHandler="appFatherHandler"></Vheader> 
```

　　b.父组件中的methods中写一个自定义的事件函数：appFatherHandler(val){}，在函数里面使用这个val，这个val就是上面子组件传过来的数据

 　看代码：

```vue
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>todolist</title>

</head>
<body>
	<div id="app">

	<App></App>
	</div>
</body>
<script src="vue.js"></script>
<script src="axios.js"></script>
<script>

	let Vheader = {
		data(){
			return {
				nav_list:['个人中心','登录','注册','username'],
				header_num:80,
			}
		},
		template:`<div>
					<h1>{{nav_list}}</h1>
					<h2>{{nav_list}}</h2>
					<button @click="zouni">go</button>
				</div>`,

		methods: {
			zouni(){
				// 给父组件传值
				this.$emit('ffunc',this.header_num);

			}
		}

	};


	let xxx = {
		data(){
			return {
				content_list:['xxx','ooo',]
			}
		},
		template:'<h1 style="color:red;">{{content_list}}</h1>',


	};

	let App = {
		data(){
			return {
				'xxx':'ooo',
				num:1001,
				son_data:'',
			}
		},
		// <Vheader ff="123"></Vheader> 静态传值
		template:
				`
				<div class="app">
					<!-- 父组件使用子组件时,写上一个自定义事件 -->
					<Vheader @ffunc="recv_data"></Vheader>
					<xxx></xxx>
					<h3>{{son_data}}</h3>

				</div>

				`,
		components: {
			Vheader,
			xxx,
		},
		methods:{
			recv_data(val){
				this.son_data = val;
			}
		}
	}


	let vm  = new Vue({
		el:'#app',

		components:{
			App,
		}


	})



</script>
</html>
```



### 5.2.3 平行组件传值

什么是平行组件，看图

![img](https://img2018.cnblogs.com/blog/988061/201904/988061-20190402001530548-257744062.png)

看代码 :声明两个全局组件T1和T2，T1组件将数据传送给T2组件

```vue
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>todolist</title>

</head>
<body>
	<div id="app">

	<App></App>
	</div>
</body>
<script src="vue.js"></script>
<script src="axios.js"></script>
<script>
	// 1 先声明一个公交车
	let bus = new Vue();

	Vue.component('t1',{
		data(){
			return {
				msg:'hello',
				num:200,
			}
		},
		template:
				`
				<div class="t1">
					<h1>{{msg}}</h1>
					<button @click="kk">zouni</button>
				</div>
				`,
		
		// created方法中的bus.$emit('xxx', this.num);不能正常放值
		// created(){
		//	
		// 	// 2. 往公交车上放值
		// 	bus.$emit('xxx', this.num);
		// },
		
		methods: {
			kk(){
				bus.$emit('xxx', this.num);
			}
		}

	});
	Vue.component('t2',{
		data(){
			return {
				msg:'hello2',
				num:1002,
				t1_num:'',
			}
		},
		template:
				`
				<div class="t2">
					<h1>{{msg}}</h1>
					<h2>{{t1_num}}</h2>
				</div>
				`,

		created(){

			bus.$on('xxx',(val)=>{
				// console.log('111111');
				console.log(val);
				this.t1_num = val;
			});


		}
	})


	let App = {
		data(){
			return {
				'xxx':'ooo',
				num:1001,
				son_data:'',
			}
		},
		// <Vheader ff="123"></Vheader> 静态传值
		template:
				`
				<div class="app">

					<h3>{{son_data}}</h3>
					<t1></t1>
					<t2></t2>
				</div>

				`,

		methods:{
			recv_data(val){
				this.son_data = val;
			}
		}
	}


	let vm  = new Vue({
		el:'#app',

		components:{
			App,
		}


	})



</script>
</html>
```



# 6. vue-router的使用

## 6.1 介绍

vue就是我们前面学习的vue基础，vue + vue-router 主要用来做SPA(Single Page Application)，单页面应用

为什么要使用单页面应用呢？因为传统的路由跳转，如果后端资源过多，会导致页面出现'白屏现象'，所以我们希望让前端来做路由，在某个生命周期的钩子函数中，发送ajax来请求数据，进行数据驱动，之前比如我们用django的MTV模式，我们是将后端的数据全部渲染给了模板，然后模板再发送给前端进行浏览器页面的渲染，一下将所有的数据都给了页面，而我们现在使用vue，我可以在组件的钩子函数中发送对应的ajax请求去获取对应的数据，而不是裤衩一下子就把数据都放到页面上了，单页面应用给我们提供了很多的便利，说起来大家可能没有什么切实的体会，来，给大家推荐一个[稀土掘金网站](https://juejin.im/)，这个网站就是一个单页面应用，是一个开发者技术社区网站，里面的资源会有很多，看样子：

　　　　![img](https://img2018.cnblogs.com/blog/988061/201904/988061-20190402231514854-561017222.png)

　　这样的网站我们通过django是可以来完成页面的渲染的，模板渲染嘛，但是这个论坛的数据资源有很多，我们通过django的MTV模式是一下子就将数据全部放到页面里面了，那么页面通过浏览器渲染的时候，浏览器可能没有那么快渲染出来，会出现几秒钟的白屏现象，也就是说几秒钟之后用户才看到页面的内容，这样体验起来就不好，为了用户体验好，就用到了我们说的单页面应用，django模板渲染做大型应用的时候，也就是页面很复杂，数据量很大的页面的时候，是不太合适的，当然如果你够nb，你也可以优化，但是一般它比较适合一些页面数据比较小的应用。

　　那么解释一下什么是单页应用，看下图：(react、angular也都是做单页面应用，很多大型的网站像网易云音乐，豆瓣等都是react写的单页面应用)

　　　　　　![img](https://img2018.cnblogs.com/blog/988061/201904/988061-20190402233838658-1651717082.png)

　　　　

　　vue + vue-router就是完成单页面应用的，vue-router(路由)是vue的核心插件



下面我们来下载一下vue-router，[文档地址](https://router.vuejs.org/zh/)，下载vue-router的cnd链接地址：https://unpkg.com/vue-router/dist/vue-router.js

官网简单操作：

```vue
// 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)

// 1. 定义 (路由) 组件。
// 下面两个组件可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
// 我们晚点再讨论嵌套路由。
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes // (缩写) 相当于 routes: routes
})

// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')

// 现在，应用已经启动了！
```



使用流程

```html
1 下载并引入vue-router的js文件: https://unpkg.com/vue-router/dist/vue-router.js
2 创建路由规则(哪个路径对应哪个组件)
	const routes = [
	  { path: '/home', component: Home },
	  { path: '/course', component: Course }
	]
3 创建对应的组件(Home\Course)
	let Home = {
		data(){
			return {
				msg:'我是home组件',
			}
		},
		template:
				`
					<div class="home">
						<h1>{{msg}}</h1>

					</div>
				`
	};
	
	let Course = {
		data(){
			return {
				msg:'我是Course组件',
			}
		},
		template:
				`
					<div class="course">
						<h1>{{msg}}</h1>

					</div>
				`

	}
4 创建vueRouter对象,并将路由规则交给这个对象
	let router = new VueRouter({
		routes,
	})
5 在vue对象中挂载一下router对象
		let vm  = new Vue({
            el:'#app',
            router,  //挂载
            components:{
                App,
            }


        })

6 创建router-link标签来指定路由
		let App = {
		data(){
			return {

				num:100,

			}
		},

		template:
				`
				<div class="app">

					<router-link to="/home">首页</router-link>
					<router-link to="/course">课程页</router-link>


					<router-view></router-view>
				</div>

				`,

		methods:{

		}
	}
7 写路由出口 router-view
```





　　　　

## 6.2 简单示例

示例： 通过不同的路径访问到不同的组件

```vue
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>todolist</title>

</head>
<body>
	<div id="app">

		<App></App>
	</div>
</body>
<script src="vue.js"></script>
<script src="axios.js"></script>
<script src="vue-router.js"></script>

<script>

	let Home = {
		data(){
			return {
				msg:'我是home组件',
			}
		},
		template:
				`
					<div class="home">
						<h1>{{msg}}</h1>

					</div>
				`

	};
	let Course = {
		data(){
			return {
				msg:'我是Course组件',
			}
		},
		template:
				`
					<div class="course">
						<h1>{{msg}}</h1>

					</div>
				`

	}


	const routes = [
	  { path: '/home', component: Home },
	  { path: '/course', component: Course }
	]

	let router = new VueRouter({
		routes,
	})

	let App = {
		data(){
			return {

				num:100,

			}
		},

		template:
				`
				<div class="app">

					<router-link to="/home">首页</router-link>
					<router-link to="/course">课程页</router-link>


					<router-view></router-view>
				</div>

				`,

		methods:{

		}
	}


	let vm  = new Vue({
		el:'#app',
		router,
		components:{
			App,
		}


	})



</script>
</html>
```





# 7. Vue自动化工具（Vue-cli）

前面学习了普通组件以后，接下来我们继续学习单文件组件则需要提前先安装准备一些组件开发工具。否则无法使用和学习单文件组件。

一般情况下，单文件组件，我们运行在 自动化工具vue-CLI中，可以帮我们编译单文件组件。所以我们需要在系统中先搭建vue-CLI工具，

官网：https://cli.vuejs.org/zh/



Vue CLI 需要 [Node.js](https://nodejs.org/) 8.9 或更高版本 (推荐 8.11.0+)。你可以使用 [nvm](https://github.com/creationix/nvm) 或 [nvm-windows](https://github.com/coreybutler/nvm-windows)在同一台电脑中管理多个 Node 版本。



nvm工具的下载和安装：   https://www.jianshu.com/p/d0e0935b150a

​                                              https://www.jianshu.com/p/622ad36ee020

安装记录:

打开:https://github.com/coreybutler/nvm-windows/releases



常用的nvm命令

nvm list   # 列出目前在nvm里面安装的所有node版本
nvm install node版本号      # 安装指定版本的node.js

例子：nvm install **14.15.1** 

nvm uninstall node版本号    # 卸载指定版本的node.js
nvm use node版本号          # 切换当前使用的node.js版本	



如果使用nvm工具，则直接可以不用自己手动下载，如果使用nvm下载安装 node的npm比较慢的时候，可以修改nvm的配置文件(在安装根目录下)

```
# settings.txt
root: C:\tool\nvm    [这里的目录地址是安装nvm时自己设置的地址,要根据实际修改]
path: C:\tool\nodejs
arch: 64
proxy: none
node_mirror: http://npm.taobao.org/mirrors/node/ 
npm_mirror: https://npm.taobao.org/mirrors/npm/
```





## 7.1 安装node.js

Node.js是一个新的后端(后台)语言，它的语法和JavaScript类似，所以可以说它是属于前端的后端语言，后端语言和前端语言的区别：

- 运行环境：后端语言一般运行在服务器端，前端语言运行在客户端的浏览器上
- 功能：后端语言可以操作文件，可以读写数据库，前端语言不能操作文件，不能读写数据库。

我们一般安装LTS(长线支持版本)：

下载地址：https://nodejs.org/en/download/【上面已经安装了nvm，那么这里不用手动安装了】



node.js的版本有两大分支：

官方发布的node.js版本：0.xx.xx 这种版本号就是官方发布的版本

社区发布的node.js版本：xx.xx.x  就是社区开发的版本



Node.js如果安装成功，可以查看Node.js的版本,在终端输入如下命令：

```
node -v

```



## 7.2 npm

在安装node.js完成后，在node.js中会同时帮我们安装一个npm包管理器npm。我们可以借助npm命令来安装node.js的包。这个工具相当于python的pip管理器。

```
npm install -g 包名              # 安装模块   -g表示全局安装，如果没有-g，则表示在当前项目安装
npm list                        # 查看当前目录下已安装的node包
npm view 包名 engines            # 查看包所依赖的Node的版本 
npm outdated                    # 检查包是否已经过时，命令会列出所有已过时的包
npm update 包名                  # 更新node包
npm uninstall 包名               # 卸载node包
npm 命令 -h                      # 查看指定命令的帮助文档
```



## 7.3 安装Vue-cli

```
npm install -g vue-cli
npm install -g vue-cli --registry https://registry.npm.taobao.org
```

如果安装速度过慢，一直超时，可以考虑切换npm镜像源：http://npm.taobao.org/

指令：

```js

 1     //临时使用
 2     npm install jquery --registry https://registry.npm.taobao.org
 3 
 4     //可以把这个选型配置到文件中，这样不用每一次都很麻烦
 5     npm config set registry https://registry.npm.taobao.org
 6 
 7     //验证是否配置成功 
 8     npm config list 或者 npm config get registry
 9 
10     //在任意目录下都可执行,--global是全局安装，不可省略
11     npm install --global cnpm 或者 npm install -g cnpm --registry=https://registry.npm.taobao.org
12 
13     //安装后直接使用
14     cnpm install jquery

```

npm和cnpm介绍

参考博客： https://blog.csdn.net/jack_bob/article/details/80644376





安装vue-cli

nvm是node.js的版本管理工具

1 安装node.js  自带npm

2 通过npm 安装vue-cli  它的运行需要依赖node.js的环境





## 7.4 使用Vue-CLI初始化创建项目

### 7.4.1 生成项目目录

使用vue自动化工具可以快速搭建单页应用项目目录。

该工具为现代化的前端开发工作流提供了开箱即用的构建配置。只需几分钟即可创建并启动一个带热重载、保存时静态检查以及可用于生产环境的构建配置的项目：

```
// 生成一个基于 webpack 模板的新项目
vue init webpack 项目名
例如：
vue init webpack myproject


// 启动开发服务器 ctrl+c 停止服务
cd myproject
npm run dev           # 运行这个命令就可以启动node提供的测试http服务器
```



那么访问一下命令执行完成后提示出来的网址就可以看到网站了：http://localhost:8080/



### 7.4.2 项目目录结构

src   主开发目录，要开发的单文件组件全部在这个目录下的components目录下

static 静态资源目录，所有的css，js，图片等文件放在这个文件夹

dist项目打包发布文件夹，最后要上线单文件项目文件都在这个文件夹中[后面打包项目,让项目中的vue组件经过编译变成js 代码以后,dist就出现了]

node_modules目录是node的包目录，

config是配置目录，

build是项目打包时依赖的目录

src/router   路由,后面需要我们在使用Router路由的时候,自己声明.







### 7.4.3 项目执行流程图

![image-20200114174249570](vue-2.assets/image-20200114174249570.png)

整个项目是一个主文件index.html,index.html中会引入src文件夹中的main.js,main.js中会导入顶级单文件组件App.vue,App.vue中会通过组件嵌套或者路由来引用components文件夹中的其他单文件组件。





# 8. 单文件组件的使用

组件有两种：普通组件、单文件组件

普通组件的缺点：

1. html代码是作为js的字符串进行编写，所以组装和开发的时候不易理解，而且没有高亮效果。
2. 普通组件用在小项目中非常合适，但是复杂的大项目中，如果把更多的组件放在html文件中，那么维护成本就会变得非常昂贵。
3. 普通组件只是整合了js和html，但是css代码被剥离出去了。使用的时候的时候不好处理。



将一个组件相关的html结构，css样式，以及交互的JavaScript代码从html文件中剥离出来，合成一个文件，这种文件就是单文件组件，相当于一个组件具有了结构、表现和行为的完整功能，方便组件之间随意组合以及组件的重用，这种文件的扩展名为“.vue”，比如："Home.vue"。

#### 

如果使用的是PyCharm这个IDE，需要我们自行下载vue的插件，在配置中找到plugins，然后搜索vue.js，点击下载，下载完成之后重启PyCharm就可以了



## 8.1创建组件

在组件中编辑三个标签，编写视图、vm对象和css样式代码。

**template 编写html代码的地方**

```vue
<template>
  <div id="Home">
    <span @click="num--" class="sub">-</span>
    <input type="text" size="1" v-model="num">
    <span @click="num++" class="add">+</span>
  </div>
</template>
```

**script编写vue.js代码**

```vue
<script>
  export default {
    name:"Home",
    data: function(){
      return {
        num:0,
      }
    }
  }
</script>
```

**style编写当前组件的样式代码**

```vue
<style scoped>
  .sub,.add{
    border: 1px solid red;
    padding: 4px 7px;
  }
</style>
```



## 8.2  完成案例-点击加减数字

创建Homes.vue

```
<template>
  <div id="Home">
    <span @click="num--" class="sub">-</span>
    <input type="text" size="1" v-model="num">
    <span @click="num++" class="add">+</span>
  </div>
</template>


<script>
  export default {
    name:"Home",
    data: function(){
      return {
        num:0,
      }
    }
  }
</script>

<style scoped>
  .sub,.add{
    border: 1px solid red;
    padding: 4px 7px;
  }
</style>
```

在App.vue组件中调用上面的组件

```
<template>
  <div id="Home">
    <span @click="num--" class="sub">-</span>
    <input type="text" size="1" v-model="num">
    <span @click="num++" class="add">+</span>
  </div>
</template>


<script>
  export default {
    name:"Home",
    data: function(){
      return {
        num:0,
      }
    }
  }
</script>

<style scoped>
  .sub,.add{
    border: 1px solid red;
    padding: 4px 7px;
  }
</style>

```

在开发vue项目之前，需要手动把 App.vue的HelloWorld组件代码以及默认的css样式，清楚。





## 8.3 组件的嵌套(上面已经写过了)

有时候开发vue项目时,页面也可以算是一个大组件,同时页面也可以分成多个子组件.

因为,产生了父组件调用子组件的情况.

例如,我们可以声明一个组件,作为父组件

在components/创建一个保存子组件的目录HomeSon



在HomeSon目录下,可以创建当前页面的子组件,例如,是Menu.vue

```vue
//  组件中代码必须写在同一个标签中
<template>
    <div id="menu">
      <span>{{msg}}</span>
      <div>hello</div>
  </div>
</template>

<script>
  export default {
    name:"Menu",
    data: function(){
      return {
        msg:"这是Menu组件里面的菜单",
      }
    }
  }
</script>

```





# 9. 在组件中使用axios获取数据

默认情况下，我们的项目中并没有对axios包的支持，所以我们需要下载安装。

在项目根目录中使用 npm安装包

```
npm install axios
```

接着在main.js文件中，导入axios并把axios对象 挂载到vue属性中多为一个子对象，这样我们才能在组件中使用。

```vue
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App' // 这里表示从别的目录下导入 单文件组件
import axios from 'axios'; // 从node_modules目录中导入包
Vue.config.productionTip = false

Vue.prototype.$axios = axios; // 把对象挂载vue中

/* eslint-disable no-new */
new Vue({
  el: '#app',
  components: { App },
  template: '<App/>'
});

```

## 9.1 在组建中使用axios获取数据

```
<script>
  export default{
	。。。
	methods:{
      get_data:function(){
         // 使用axios请求数据
        this.$axios.get("http://wthrcdn.etouch.cn/weather_mini?city=深圳").then((response)=>{
            console.log(response);
        }).catch(error=>{
            console.log(error);
        })
      }
    }
  }
</script>
```





使用的时候，因为本质上来说，我们还是原来的ajax，所以也会收到同源策略的影响。



# 10. 引入ElementUI

对于前端页面布局，我们可以使用一些开源的UI框架来配合开发，Vue开发前端项目中，比较常用的就是ElementUI了。

ElementUI是饿了么团队开发的一个UI组件框架，这个框架提前帮我们提供了很多已经写好的通用模块，我们可以在Vue项目中引入来使用，这个框架的使用类似于我们前面学习的bootstrap框架，也就是说，我们完全可以把官方文档中的组件代码拿来就用，有定制性的内容，可以直接通过样式进行覆盖修改就可以了。

中文官网：[Element - 网站快速成型工具](https://element.eleme.cn/2.13/#/zh-CN)

文档快速入门：http://element-cn.eleme.io/#/zh-CN/component/quickstart



## 10.1 快速安装ElementUI

项目根目录执行以下命令:

```
npm i element-ui -S --registry https://registry.npm.taobao.org
```

上面的命令等同于 `npm install element-ui --save`



## 10.2 配置ElementUI到项目中

在main.js中导入ElementUI，并调用。代码：

```javascript
// elementUI 导入
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';  // 需要import引入一下css文件，和我们的link标签引入是一个效果，而import .. from ..是配合export default来使用的
// 调用插件
Vue.use(ElementUI);
```



成功引入了ElementUI以后，接下来我们就可以开始进入前端页面开发，首先是首页。





