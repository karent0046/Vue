# 组件

## 1、定义组件

Vue 自定义组件分为两种：全局注册和局部注册，全局组件可以在任何地方引用，局部组件只能在当前 Vue 实例使用。

### 1）全局注册

使用Vue.component(tagName, options)来定义：
注意：HTML 特性是不区分大小写的，所有在定义组件时尽量使用中划线“-”来指定组件名。
即使，使用了驼峰标示命名如：myComponent，在页面引用时仍然要使用<my-component>进行引用。
案例:
	
```
<div id="app">
		<!--使用组件-->
	<my-hello></my-hello>
			
</div>
</body>
	<script type="text/javascript">
		
		new Vue({
			el:"#app"
		});
		
		/*定义全局组件*/
		Vue.component("my-hello",{
			template:"<h3>Hello Vue</h3>"
		});
		
	</script>
  ```
  
### 2）局部注册
在 Vue 实例中使用 components 属性来定义
```
<div id="app">
 <!-- 使用组件 -->
 <my-component></my-component>
 <inner-component></inner-component>
</div>
<script>
 /*定义全局组件*/
 Vue.component('my-component',{
 template:'<h4>我是自定义组件</h4>'
 });
 /***
 * 此种定义方式全局注册,在任何地方都可以通过自定义标签名进行引用
 * */
 var app = new Vue({
 el:'#app',
 //使用 components 关键字
 components:{
 'inner-component':{
 template:'<h4>我是局部注册组件</h4>'
 }
 }
 });
 </script>

 ```
 
 ### 3)is 属性
在 table 标签中直接使用自定义组件，无法正常显示。DOM 解析时会解析到<table>标签的外部：
原因是：table/ol/ul/select 这种 html 标签有特殊的结构要求,不能直接使用自
定义标签。他们有自己的默认嵌套规则，比如:
	
table> tr> [th, td];
	
ol/ul > li;

select > option

解决上述问题，可以使用 is 进行标签转换，形式：

```< is=”my-component”>```

案例:

```
<table id="app">
 <!-- 使用组件 -->
<!-- <my-component></my-component>-->
 <!-- 使用 is -->
 <tr is="my-component"></tr>
</table>
<script>
 /***
 * 原因是: table/ol/ul/select 这种特殊结构要求,不能使用自定义标签:
 * 比如: table> tr> [th, td]; ol/ul > li; select > option
 * 可以使用 is 进行标签转换
 * */
 /*定义组件 1*/
 Vue.component('my-component',{
 template:'<h4>我是自定义组件</h4>'
 });
 var app = new Vue({
 el:'#app'
 });
</script>
```
### 4)模板

当模板的html结构比较复杂时，直接在template属性中定义就不现实了，效率也会很低，此时我们可以使用模板，定义模板的四种形式:

1)直接使用字符串定义

2)使用<script type="text/x-template">
	
3)使用<template>标签
	
4)使用.vue组件，需要使用模块加载机制

在使用直接字符串模板时、x-template和.vue组件时,不需要is进行转义。

### 5)data属性

通过 data 属性指定自定义组件的初始数据，要求 data 必须是一个函数，如果不是函数就会报错。

案例:
```
<script type="text/javascript">
		/*定义组件需要在实例化vue之前*/
		Vue.component("my-hello",{
			template:"<button @click='count++'>按钮{{count}}</button>",
			// 定义组件内部data: 必须通过函数定义
			data:function(){
				return {count:0};
			}
		});
		
		
		new Vue({
			el:"#app",
			data:{
				
			}
		});
		
	</script>
```

### 6)Props属性

组件可以嵌套使用，叫做父子组件。那么父组件经常要给子组件传递数据这叫做父子组件通信。

父子组件的关系可以总结为 props 向下传递，事件向上传递。

父组件通过 props 给子组件下发数据，子组件通过事件给父组件发送消息。
				
1、在父组件中定义数据

2、在使用组件时，绑定父组件中的数据

3、在子组件中通过props属性声明父组件中传递过来的参数

4、在template属性中使用父组件中的参数

案例:
```
<script type="text/javascript">
		/*定义组件需要在实例化vue之前*/
		Vue.component("my-hello",{
			// 声明父组件传递过来的参数
			props:["txt1","txt2"],
			template:"<div>{{txt1}}：{{txt2}}</div>"
		});
		
		
		new Vue({
			el:"#app",
			data:{
				msg:"来自系统的消息",
				txt:"Hello Vue!"
			}
		});
		
	</script>
```
### 7)props校验

子组件在接收父组件传入数据时, 可以进行props校验，来确保数据的格式和是否必传。可以指定一下属性：

1) type: 指定数据类型 String Number Object ...注意不能使用字符串数组，只能是对象大写形式

2) required: 指定是否必必须输入

3) default: 给默认值或者自定义函数返回默认值

4) validator: 自定义函数校验	


案例
```
<div id="app">
			<!--使用组件-->
			<my-hello class="item" style="font-size: 30px;color:pink;"  ></my-hello>
		</div>
		
		
	</body>
	
	<script type="text/javascript">
		/*定义组件需要在实例化vue之前*/
		Vue.component("my-hello",{
			template:"<span class='test' style='color:red'>Vue</span>"
		});
		
		
		new Vue({
			el:"#app"
			
		});
```

注意:
**非props属性:引用子组件时，非定义的props属性,自动合并到子组件上,class和style也会自动合并。**

案例:

```
<script type="text/javascript">
		/*定义组件需要在实例化vue之前*/
		Vue.component("my-hello",{
			// 声明父组件传递过来的参数
			// props:["txt1","txt2"], // 数组不能做校验
			// 对象可以做校验
			props:{
				// 基础类型检测 (`null` 指允许任何类型)
				txt1:[String, Number], // 可以支持多个
				txt2:String,
				// 必传且是字符串
				txt3:{
					required:true,
					type:String
				},
				// 数值且有默认值
				txt4:{
					type:Number,
					default: 100
				},
				// 自定义验证函数
				txt5:{
					validator:function(value){
						return value > 10;
					}
				}
			},
			template:"<div>{{txt1}}：{{txt2}}---{{txt3}}---{{txt4}}----{{txt5}}</div>"
		});
		
		
		new Vue({
			el:"#app",
			data:{
				msg:"来自系统的消息",
				txt:"Hello Vue!",
				msg2:"Admin",
				num:10,
				money:19
			}
		});
		
	</script>
```

## 2.自定义事件

父组件给子组件传值使用props属性, 那么需要子组件更新父组件时,要使用自定义事件$on和$emit：

$on监听: 不能监听驼峰标示的自定义事件, 使用全部小写(abc)或者-(a-b-c)

$emit主动触发: $emit(事件名,传入参数)

主动挂载
自定义事件不仅可以绑定在子组件，也可以直接挂载到父组件，使用$on绑定和$emit触发。

```
<div id="app">
			<my-hello v-on:update-count="changecount()"></my-hello>
			{{count}}
		</div>
		
		
	</body>
	
	<script type="text/javascript">
		
		Vue.component("my-hello",{
			template:"<button v-on:click='update'>子组件Child</button>",
			methods:{
				update:function(){
					console.log("点击...");
					this.$emit("update-count","自定义事件");
				}
			}
		});
		
		
		var app = new Vue({
			el:"#app",
			data:{
				count:0
			},
			methods:{
				changecount:function(){
					this.count++;
				}
			}
		});
		// 主动挂载自定义事件
		app.$on("update-count",function(value){
			console.log(value);
			this.count++;
		});
		// 触发自定义事件
		app.$emit("update-count","这是自定义事件");
		
	</script>
```

## 3.插槽分发

### 1)slot插槽

父子组件使用时,有时需要将父元素的模板跟子元素模板进行混合,这时就要用到slot 插槽进行内容分发.

简单理解就是在子模板中先占个位置<slot>等待父组件调用时进行模板插入。

案例:
```
		<div id="app">
			<my-hello></my-hello>
			<my-hello>
				<h3>你好</h3>
				<p>这是p元素</p>
			</my-hello>
		</div>
		
		<!--使用template标签-->
		<template id="tpl1">
			<div>
				<h4>Hello Vue</h4>	
				<!--插槽，占位-->
				<slot>如果没有传递数据，默认显示这段文本</slot>
			</div>
		</template>
		
	</body>
	
	<script type="text/javascript">
		
		// 自定义组件
		Vue.component("my-hello",{
			template:"#tpl1",
		});
		
		
		var app = new Vue({
			el:"#app"
			
		});
```

