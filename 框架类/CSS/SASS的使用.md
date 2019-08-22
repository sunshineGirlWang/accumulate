
### 一、.sass与.scss后缀文件的区别
	(1).scss是 Sass 3 引入新的语法，其语法完全兼容 CSS3。

	(2).sass 是以严格的缩进式语法规则来书写，不带大括号({})和分号(;)，而 .scss 的语法书写和我们的 CSS 语法书写方式非常类似。

	(3)推荐使用.scss的书写方式。

### 二、Sass中的注释有3种：

		1、//我是单行注释
		不会出现在编译之后任何格式的CSS文件中。

		2、/*我是多行注释*/
		只会出现在编译之后代码格式为expanded的CSS文件中。

		3、/*!我是强制注释*/
		会出现在任何代码格式的CSS文件中。


### 三、常用语法
详细教程:  
>https://www.sass.hk/docs/

1、变量使用**$**定义

		$background-color: #f0f0f0;

		.test{
			background: $background-color;
		}

2、可支持选择器嵌套和属性嵌套。
	hover的伪类，就需要用到 **&** 这个连接父选择器的标识符。

	(1)选择器嵌套
		div{
			font-size: 20px;

			p{
				margin: 5px;
			}

			a{
				color: red;
				&:hover{
					color: pink;
				}
			}
		}

	(2)属性嵌套
		div {
			border: {
			style: solid;
			width: 1px;
			color: #ccc;
			}
		}


3、sass中，选择器继承可以让选择器继承另一个选择器的所有样式，并联合声明。

   使用选择器的继承，要使用关键词 **@extend** ，后面紧跟需要继承的选择器。

		.class1 {
			border: 1px solid #333;
		}

		.class2 {
			@extend .class1;
			background-color: #999;
		}


4、sass中使用@mixin声明混合，可以传递参数，参数名以$符号开始，多个参数以逗号分开，也可以给参数设置默认值。

声明的@mixin通过 **@include+minxin名称** 来调用。


(1)无参数mixin声明及调用：

		@mixin mixName {        
			float: left;
			margin-left: 10px;
		}

		div {
			@include mixName;
		}


(2)带参数mixin声明及调用

可以不给参数值直接写参数，如果给了值的话，就是参数的默认值，在调用的时候传入其他值就会把默认值覆盖掉。

		@mixin left($value: 10px) {
			float: left;
			margin-left: $value;
		}
		div {
			@include left(66px);
		}


(3)带多组数值参数的mixin声明及调用

如果一个参数可以有多组值，如box-shadow、transition等，那么参数则需要在变量后加三个点表示，如$variables...。

		@mixin mixName($shadow...) {
		box-shadow:$shadow;
		}
		.box{
		  	@include mixName(0 2px 2px rgba(0,0,0,.3),0 3px 3px rgba(0,0,0,.3),0 4px 4px rgba(0,0,0,.3));
		}


5、使用 **@import** 引入文件