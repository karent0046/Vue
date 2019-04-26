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
 
 
