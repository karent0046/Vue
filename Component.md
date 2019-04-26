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
  
