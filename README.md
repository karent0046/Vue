# Vue

##简介
Vue.js (读音 /vjuː/，类似于 view) 是一套构建用户界面的渐进式框架。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。
Vue 的核心库只关注视图层，它不仅易于上手，还便于与第三方库或既有项目整合。
Vue.js 提供了 MVVM (Model-View-ViewModel 的简写)数据绑定和一个可组合的组件系统，具有简单、灵活的 API。
MVVM: 它采用双向绑定（data-binding）：View 的变动，自动反映在ViewModel，反之亦然。

## 一.Vue 模板语法
如何使用Vue：

1、引入Vue的核心JS文件

2、准备Dom结构

3、实例化组件

通过el属性，挂载元素，绑定id为app的html元素
  
通过data属性，定义数据，可以在html代码段中显示的数据
  
4、获取数据

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值
  
案例:

```
<!--1、引入Vue的核心JS文件，Vue会被当做全局变量使用-->
		<script type="text/javascript" src="js/vue.js" ></script>
<!--2、准备Dom结构-->
		<div id="app">
			<!--双花括号取值-->
			{{msg}}
		</div>
		
		<script type="text/javascript">
			/*3、实例化组件*/
			var app = new Vue({
				el:"#app", // el：element的简写。挂载元素，绑定id为app的html元素
				data:{ // 定义数据，可以在html代码段中显示的数据
					msg:"Hello Vue！"
					
				}
			});
		</script>
 ```
### 1.插值
数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：
```
<div id="app">{{message}}</div>
```

### 2.JavaScript 表达式

在 Vue 模板中，提供了完全的 JavaScript 表达式支持。
**注意，只能在模板中写入表达式，而不能使用 js 语句，所以下面的写法会报错:**

案例
```
<div id="app">
		
			<div>{{msg}}</div>
			<div>{{msg.split("").reverse().join("")}}</div> <!--字符串方法操作-->
			<div>{{num + 1}}</div> <!--数值运算-->
			<div>{{flag?"喜欢":"不喜欢"}}</div> <!--三目运算符-->
			<!--下面的会报错-->
			<!-- 这是语句，不是表达式 -->
			<!--{{ var a = 1 }}-->
			<!-- 流控制也不会生效，请使用三元表达式 -->
			<!--{{ if (ok) { return message } }}-->
		</div>
		
		<script type="text/javascript">
			
			var app = new Vue({
				el:"#app", // el：element的简写。挂载元素，绑定id为app的html元素
				data:{ // 定义数据，可以在html代码段中显示的数据
					msg:"Hello Vue！",
					num:10,
					flag:true
				}
			});
 ```
 **Vue中不能用if流程控制语句只能用三目运算符代替**
 
 ## 二.指令
 指令 (Directives) 是带有 v- 前缀的特殊属性。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。
### 1.文本渲染
v-text:
	显示数据，响应式（默认）
	简写：{{}}
v-once:
	只渲染元素和组件一次。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。
v-html:
	可以显示html元素
案例:
```
<div id="app">
			<div v-text="msg"></div>
			<!--简写-->
			<div>{{msg}}</div>
			<hr />
			<div v-once>{{txt}}</div>
			<hr />
			<div>{{tpl}}</div>
			<div v-html="tpl"></div>
		</div>
	</body>
	<script type="text/javascript">
		var app = new Vue({
			el:"#app",
			data:{
				msg:"Hello",
				txt:"Hello Vue",
				tpl:"<h2>上海</h2>"
			}
		});
	</script>
```
### 2.class 与 style 绑定

v-bind
v-bind 指令可以绑定元素的属性，动态给属性赋值。比如：v-bind:class、v-bind:style、v-bind:href 形式。
v-bind 的简写形式：v-bind: 简化为 : ，上边的形式可以改写成：:class、:style、:href
案例:
```
<div id="app">
			<div title="你好">Hello</div>
			<hr />
			<div v-bind:title="msg">你好，我是大尤，你觉得Vue怎么样？</div>
			<div :title="msg">你好，我是大尤，你觉得Vue怎么样？</div>
			<hr />
			<img :src="src"/>
			<a :href="href">Vue</a>
		</div>
	</body>
	<script type="text/javascript">
		var app = new Vue({
			el:"#app",
			data:{
				msg:"Vue is very good",
				src:"https://cn.vuejs.org/images/logo.png",
				href:"https://cn.vuejs.org/"
				
			}
		});
	</script>
  ```
### 3.class绑定

绑定DOM对象的class样式有以下几种形式：

绑定多个class

使用对象classObject绑定

使用数组classList绑定

绑定的对象可以同时切换多个class

对象和数组绑定的区别：

对象可以控制class的添加和删除；数组不能控制删除

案例:
```
<div id="app">
			<div class="red green item">Vue</div>
			<hr />
			<div :class="{red:true,green:false,item:true}">Vue</div>
			<div :class="classObj">Vue</div>
			<div :class="classList">Vue</div>
		</div>
	</body>
	<script type="text/javascript">
		var app = new Vue({
			el:"#app",
			data:{
				classObj:{
					red:true,
					green:false,
					item:true
				},
				classList:["red","item","test"]
			}
		});
	</script>
```
### 4.style绑定

style绑定

绑定形式跟class一致：

使用内联对象Object

直接传入对象styleObject

使用数组对象styleList

案例:
```
<div id="app">
			<div style="color:red;font-size: 30px;">我是文本</div>
			<div :style="{'color':'red','font-size':'30px'}">我是文本</div>
			<div :style="styleObj">我是文本</div>
			<div :style="[styleObj,styleObj2]">我是文本</div>
		</div>
	</body>
	<script type="text/javascript">
		var app = new Vue({
			el:"#app",
			data:{
				styleObj:{
					'color':'red',
					'font-size':'30px',
					'font-family':'楷体'
				},
				styleObj2:{
					'background':'pink'
				}
			}
		});
	</script>
```
### 5.监听事件

绑定事件

语法：

v-on:事件名

简写：

@事件名

案例:
```
<div id="app">
			count:{{count}} &nbsp;
			<button v-on:click="count++">add</button>
			<button @click="count--">down</button>
		</div>
	</body>
	<script type="text/javascript">
		var app = new Vue({
			el:"#app",
			data:{
				count:0
			}
		});
	</script>
```

