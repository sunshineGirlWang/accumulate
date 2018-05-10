### 记录一下React开发过程中遇到的一些问题
##### 好记性不如烂笔头^0^
##### Author：wang-qingqing

1. render()里面只能return一个JSX，

   因此在使用数组的.map()方法时，每次循环都要return一个JSX。

2. 在使用数组的.map()方法时，建议使用ES6的箭头函数，避免出现this指向的问题。

		oneArray.map((item,index)=>{
			return (
				<a onClick={this.play.bind(this)}>test</a>
			)
		})


