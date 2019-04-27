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


