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
			onClick={(value) => this.test('key',value)}

		(3)语句很少时
			onClick={() => this.state.triggle = !this.state.triggle}

4. 父组件调用子组件的方法：

		class Parent extends React.Component{
			<Child  onRef={(ref) => this.child = ref} />

			test = () => {
				this.child.onchange();
			}
		}

		class Child extends React.Component{
			componentDidMount(){
				this.props.onRef = this;
			}

			onchange = () => {
				alert('child');
			}
		}

5. es6中，寻找数组中是否包含某个元素，
	let arr = [1,2,3,-5]
	arr.find((n) => n<0)  //-5
	arr.findIndex((n) => n<0)  //3 

	注意:indexOf方法无法识别数组的NaN成员，但是findIndex可以通过Object.is方法做到。

		[NaN].findIndex(y => Object.is(NaN,y))

6. 使用this.forceUpdate()来更新当前组件的render()方法。
	
7. 如果两个兄弟组件A和B，A想调用B组件的方法，必须通过两兄弟的父组件C来调用。
	例子待补充。
	
8. 将一个形如"a:b:c"的字符串转换成数组，其中a、b、c为整型，转换后的数组内也是整数。

		Array.from("1:3:5".split(":"),(value) => Number(value))

9. 