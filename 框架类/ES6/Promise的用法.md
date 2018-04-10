1、Promise对象

	A.主要用于异步计算。
    B.可以将异步操作队列化，按照期望的顺序执行，返回符合预期的结果。
    C.可以在对象之间传递和操作Promise。

2、有了node.js之后，对异步的依赖加剧了:

	A.无阻塞高并发，是node.js的招牌；
    B.异步操作是其保障；
    C.大量操作依赖回调函数。

3、第一个参数是error信息，第二个参数才是回调函数。

4、回调有四个问题：

	A.嵌套层次很深，难以维护；
    B.无法正常使用return和throw；
    C.无法正常检索堆栈信息；
    D.多个回调之间难以建立联系。
    
5、Promise：

	A.可以很好的解决回调嵌套问题；
    B.代码阅读体验很好；
    C.不需要新的语言元素。
    
6、语法：

	new Promise(
    	/*执行器 executor*/
        function(resolve,reject){
        	//一段耗时很长的异步操作
            resolve();//数据处理完成
            reject();//数据处理出错
        }	
    )
    .then(function A(){
    	//成功，下一步
    }，function B(){
    	//失败，做相应处理
    })
    
7、关于Promise的相关说明：

	A.Promise是一个代理对象，它和原先的操作并无关系；
    B.Promise有三个状态：
    	（1）pending[待定]初始状态；
        （2）fulfilled[实现]操作成功；
        （3）rejected[被否决]操作失败。
    C.Promise实例一经创建，执行器立即执行。
    D.Promise状态发生变化，就会触发.then()执行后续步骤。
    E.Promise状态一经改变，不会再变。

8、一些示例的代码：

	//timeout.js
    console.log('here we go');
    new Promise( resolve => {
    	setTimeout( ()=> {
    		resolve('hello');
    	},2000);
    })
     .then( value => {
     	console.log(value + 'world');
     })

     //timeout2.js
    console.log('here we go');
    new Promise( resolve => {
    	setTimeout( ()=> {
    		resolve('hello');
    	},2000);
    })
     .then( value => {
     	console.log(value);
     	return new Promise( resolve => {
     		setTimeout( () => {
     			resolve('world')
     		},2000);
     	})
     })
      .then( value => {
      	console.log( value + 'world');
      })

    //fulfilled-then.js
    console.log('start');
    let promise = new Promise(resolve => {
    	setTimeout( () => {
    		console.log('the promise fulfilled');
    		resolve('hello,world');
    	},1000)
    })

    setTimeout( () => {
    	promise.then( value => {
    		console.log(value);
    	})
    },3000);


    //timeout3.js
    console.log('here we go');
    new Promise( resolve => {
    	setTimeout( ()=> {
    		resolve('hello');
    	},2000);
    })
     .then( value => {
     	console.log(value);
     	console.log('everyone');
     	// return new Promise( resolve => {
     	// 	setTimeout( () => {
     	// 		resolve('world')
     	// 	},2000);
     	// })
     	return false;
     })
      .then( value => {
      	console.log( value + 'world');
      })
  
9、关于.then()的说明：

	A. .then()接受两个函数作为参数，分别代表fulfilled和rejected；
    B. .then()返回一个新的Promise实例，所以它可以链式调用；
    C.当前面的Promise状态改变时，.then()根据其最终状态，选择特定的状态响应函数执行；
    D.状态响应函数可以返回新的Promise，或其他值；
    E.如果返回新的Promise，那么下一级.then()会在新Promise状态改变之后执行
    F.如果返回其它任何值，则会立刻执行下一级。
    
10、.then()里有.then()的情况

	因为.then()返回的还是Promise实例。会等里面的.then()执行完，再执行外面的。
    对于我们来说，此时最好将其展开，会更好读。
    
11、问题：下面的四种promise的区别是什么
	
    
12、错误处理

	Promise会自动捕获内部异常，并交给rejected响应函数处理。
    通常有两种做法：
    	A.reject('错误信息')
        	.then(null,message => {})
        B.throw new Error('错误信息')
        	.catch( message => {})
    推荐使用B方案，更加清晰、好读，并且可以捕获前面的错误。

13、注意：强烈建议在所有队列最后都加上.catch()，以避免漏掉错误处理造成意想不到的问题。

		doSomething()
			.doAnotherThing()
			.doMoreThing()
			.catch( err => {
				console.log(err);
			})
            
14、Promise.all([p1,p2,p3,...])用于将多个Promise实例，包装成一个新的Promise实例。

	A.它接受一个数组作为参数；
    B.数组里可以是Promise对象，也可以是别的值，只有Promise会等待状态改变；
    C.当所有子Promise都完成，该Promise完成，返回值是全部值的数组；
    D.有任何一个失败，该Promise失败，返回值是第一个失败的子Promise的结果。

15、Promise.all()最常见的就是和 .map()连用。

16、实现队列

	有时候我们不希望所有动作一起发生，而是按照一定顺序，逐个进行。
    A.使用 .forEach()
    	




