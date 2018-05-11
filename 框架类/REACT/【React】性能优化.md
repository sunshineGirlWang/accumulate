###  一、【react】利用shouldComponentUpdate钩子函数优化react性能以及引入immutable库的必要性
>https://www.cnblogs.com/penghuwan/p/6707254.html

#### 【1】问题导入
1. setState()函数在任何情况下都会导致组件重渲染吗？如果setState()中参数还是原来没有发生任何变化的state呢？

2. 如果组件的state没有变化，并且从父组件接受的props也没有变化，那它就一定不会重渲染吗？

3. 如果1，2两种情况下都会导致重渲染，我们该如何避免这种冗余的操作，从而优化性能？

#### 【2】具体分析：
1. setState()函数在**任何情况下都会导致组件重渲染**，即使没有导致state的值发生变化的setState也会导致重渲染。

2. **shouldComponentUpdate**函数是重渲染时，render()函数调用前被调用的函数。

        (1)接受两个参数：nextProps和nextState，分别表示下一个props和下一个state的值。
        (2)并且当函数返回false的时候，阻止接下来的render()函数调用，阻止组件重渲染；
        当函数返回true时，组件照常重渲染。
        (3)代码示例：
            //在render函数调用前判断：如果前后state中Number不变，通过return false阻止render调用
            shouldComponentUpdate(nextProps,nextState){
                if(nextState.Number == this.state.Number){
                    return false
                }
            }


3. 组件的state没有变化，并且从父组件接受的props也没有变化，那它还是**可能**会重渲染的。

4. 解决第3点，还是要用到shouldComponentUpdate这个钩子函数的。

        shouldComponentUpdate(nextProps,nextState){  //number如果没有变化，组件就不重渲染了。
            if(nextProps.number == this.props.number){
                return false
            }
            return true
        }
    
    **注意**: nextProps.number == this.props.number 不能写成nextProps == this.props

    （nextProps == this.props总会返回false，因为它们是堆中内存不同的两个对象）。


5. 总结：

    前后不改变state值的setState(理论上)和无数据交换的父组件的重渲染都会导致组件的重渲染，

    但是可以在shouldComponentUpdate这里阻止这种浪费性能的行为。

6. 如果是对象，直接使用nextProps.numberObject.number == this.props.numberObject.number时，就会出现问题（内存存储机制导致的）。

7. 对于第6点的解决办法是：

    (1)ES6的扩展语法Object.assign()    //react推荐的es6写法
    (2)深拷贝/浅拷贝或利用JSON.parse(JSON.stringify(data))     //相当于深拷贝，但使用受一定限制
    (3)immutable.js    //react官方推荐使用的第三方库
    (4)继承react的PureComponent组件

8. 




    

