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
## 子组件  
可以通过`this.props.children`获取当前子组件，也就是一下代码中的span节点，也可以通过React.Children来获取所有子组件。  
```javascript
var HeloMan = React.createClass({
		render :function(){
		return (<ol>{
						React.Children.map(this.props.children,function(child){
							return <li>{child}</li>
						})
				}</ol>)
		}
	})
	ReactDOM.render(
		<HeloMan>
			<span>heihei</span>
			<span>heihe</span>
			<span>heihi</span>
		</HeloMan>,
	document.querySelector('#id'))
```
## 属性约定  
父组件可以通过props给子组件传值，但假如我们要给同事写子组件，而同事不知道prop的类型，  
就会产生不必要的错误、降低开发效率。所以为了组件的健全性，我们可以通过propType指定prop的类型以此  
类约束变量类型。  
```javascript
var HeloMan = React.createClass({
		getDefaultProps: function(){
			return {
				title: 'hello man'
			}
		},
		propTypes: {
			title : React.PropTypes.string.isRequired
		},
		render :function(){
			return (<h1>{this.props.title}</h1>)
		}
	})
	let data = '5';
	ReactDOM.render(<HeloMan title={data}></HeloMan>,
	document.querySelector('#id'))
```  
以上代码中，title属性的值必须是字符串，如果是其他类型，则控制台会报错。以此来约束属性的类型。  
你也可以看到上述代码有一个getDefaultProps属性，他也给属性指定一个默认值，如果未设置该属性，则采用默认值。  
## 真实节点  
像vue和react这类的框架，为了减少DOM操作数，都会在改变DOM前构建虚拟DOM，从而当数据发生改变时，先修改虚拟DOM，监控DOM树的变化批量的修改DOM，从而减少DOM操作，增加网页性能。所以当数据改变的时候，真实的DOM节点有可能并未改变。因此如果我们需要获取真实节点就要注意到这一点。那么怎么在react中获取真实节点呢。React提供了一个叫做ref的属性。    
```javascript
	var HeloMan = React.createClass({
		callMyName: function(){
			console.log(this.refs.callname);
		},
		render :function(){
			return (<h1 ref='callname' onClick={this.callMyName}>{this.callMyName()}my name is duhao</h1>)
		}
	})

	let data = '5';
	ReactDOM.render(
		<HeloMan>
		</HeloMan>,
	document.querySelector('#id'))
```
在以上代码中，子组件定义了callMyName事件，并且在组件内部绑定click事件调用该事件，在事件内部用refs获取真实节点。  
找到ref属性对应的节点。因为当click的时候节点肯定已经添加完成，所以可以得到预期结果，而在组件的text中，同样的调用了这个事件，但却打印出undefined，这是以为刚开始的时候真实节点还没有构建完成。  
## 组件状态  
在react中，组件是可变的，当组件的属性发生变化时，组件的render函数就会重新绘制一遍。  
```javascript
	var HeloMan = React.createClass({
		getInitialState() {
			return {
				onoff : true
			}
		},
		changeLight() {
			this.setState({
				onoff : !this.state.onoff
			})
		},
		render :function(){
			var lightState = this.state.onoff?'turn down': 'turn up';
			return (<h1 ref='callname' onClick={this.changeLight}>{lightState}</h1>)
		}
	})
	ReactDOM.render(
		<HeloMan>
		</HeloMan>,
	document.querySelector('#id'))
```  
getInitialState函数用来初始化属性。而用setState来通知render发生重新渲染。所以你手动的改变state的状态是无效的。  
## 组件的生命周期
react组件的生命周期相比vue要明了很多了。  
```javascript
	var HeloMan = React.createClass({
		componentWillMount() {
			console.log('componentWillMount')
			console.log(document.querySelector('#id h1'))
		},
		componentDidMount() {
			console.log('componentDidMount')
			console.log(document.querySelector('#id h1'))
		},
		componentWillUpdate() {
			console.log('componentWillUpdate')
			console.log(this.state.onoff);
		},
		componentDidUpdate() {
			console.log('componentDidUpdate')
			console.log(this.state.onoff);
		},
		componentWillUnmount(){
			console.log('componentWillUnmount')
		},
		getInitialState() {
			return {
				onoff : true
			}
		},
		changeLight() {	
			this.setState({
				onoff : !this.state.onoff
			})	
		},
		render :function(){
			var lightState = this.state.onoff?'turn down': 'turn up';
			return (<h1 ref='callname' onClick={this.changeLight}>{lightState}</h1>)
		}
	})
	ReactDOM.render(
		<HeloMan>
		</HeloMan>,
	document.querySelector('#id'))
```







