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

3. 在写一个onClick的时候，如果这个function中没有用到this.state或者this.props时，

	建议不要使用this.test.bind(this)这种形式的写法，因为都要重新渲染组件，影响性能。

	常用的几种写法有:

		(1)没有入参时
			onClick={this.test}

		(2)有入参时
			onClick={this.test('1')}

		(3)语句很少时
			onClick={() => this.state.triggle = !this.state.triggle}

4. 
