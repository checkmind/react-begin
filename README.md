# react-begin
刚找到新工作，工作中可能会用到react，所以入门一下react。  
主要参考：[链接](http://www.ruanyifeng.com/blog/2015/03/react.html)  
# 浏览器端使用react  
首先引用三个js文件  
```html
	<script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
```  
其中browser是babel以支持react的jsx语法。  
我们的代码写在script标签里，但是type要声明为 babel
```javascript
<script type="text/babel">
	ReactDOM.render(<h1>HELLO WORLD</h1>,document.querySelector('#id'))
</script>
```
然后浏览器打开1.html，render函数就将html代码渲染至id元素中。  
## 循环
```javascript
	let arr = ['heihei','hehe','lala'];
	ReactDOM.render(
	<div>{
		arr.map(function(val){
			return <li>hello {val}</li>
		})
	}</div>,
	document.querySelector('#id'))
```
render函数内部可以插入html片段。使用map循环来渲染一个列表  
## 数组  
如果直接填充数组，则render将渲染数组的所有成员。  
## 组件  
```javascript
	var HeloMan = React.createClass({
			render :function(){
				return <div >HELLO {this.props.name}</div>;
			}
	})
	ReactDOM.render(
	<HeloMan name='duhao' />
	,
	document.querySelector('#id'))
```
可以用React.createClass定义一个组件。然后在render函数中引用组件名，组件名第一个字母必须大写。否则将没有效果。  
并且组件内顶层元素只能存在一个，如果大于一个则报错。  
比如下面的代码会报错：
```javascript
var HeloMan = React.createClass({
			render :function(){
				return <div >HELLO {this.props.name}</div><a></a>;
			}
	})
//embedded: Adjacent JSX elements must be wrapped in an enclosing tag
```






