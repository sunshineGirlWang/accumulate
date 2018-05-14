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

8. ES6的扩展语法Object.assign()

    Object.assign(TargetObj,obj1,obj2...)[返回值为Object]

    可将obj1,obj2等组合到TargetObj中并返回一个和TargetObj值相同的对象。

        let obj = Object.assign({},{a:1},{b:1})  //obj为{a:1,b:1}

9. immutable.js
    (1)为什么基本类型和引用类型在变量赋值不同？
        因为基本类型变量占用的内存很小，而引用类型变量占用的内存比较大，
        
        几个引用类型变量通过指针共享同一个变量可以节约内存。
    
    (2)通过npm install immutable安装。

        const {fromJS} = require('immutable');
        let obj1 = fromJS({name: 'aaa'}),obj2;
        obj2 = obj1;//obj2取得与obj1相同的值，但两个引用指向不同的对象
        obj2 = obj2.set('name','ccc');
        console.log(obj1.get('name'));   //aaa
        console.log(obj2.get('name'));   //ccc

    (3)对于immutable对象，必须使用immutable提供的API：
        A.fromJS(obj):
            把传入的obj封装成immutable对象，在赋值给新对象时传递的只有本身的值，而不是指向内存的地址。
        B.obj.set(属性名,属性值):
            给obj增加或修改属性，但obj本身并不变化，只返回修改后的对象。
        C.obj.get(属性名): 
            从immutable对象中取得属性值。
        
    (4)优点：
        深拷贝/浅拷贝本身是很耗内存，而immutable本身有一套机制使内存消耗降到最低

    (5)缺点：
        你多了一整套的API去学习，并且immutable提供的set,map等对象容易与ES6新增的set,map对象弄混

10. 继承react的PureComponent组件
    如果只是单纯地想要避免state和props不变时的冗余的重渲染，
    
    那么react的pureComponent可以非常方便地实现这一点：

        import React,{PureComponent} from 'react'; 
        class YouComponent extends PureComponent{
            render(){
                //...
            }
        }


    

### 二、谈一谈创建React Component的几种方式
>https://www.cnblogs.com/Unknw/p/6431375.html

#### 【1】几种方法
1. createClass

        var React = require("react");
        var Greeting = React.createClass({
            propTypes: {
                name: React.PropTypes.string  //属性校验
            },
            getDefaultProps: function(){
                return {
                    name: 'Roy'   //默认属性值
                }
            },
            getInitialState: function(){  //初始化state
                return {
                    count: this.props.initialCount
                }
            },
            handleClick: function(){
                //处理的函数
            },
            render: function(){
                return <h1>Hello,{this.props.name}</h1>
            }
        })
    
    在createClass中，React对属性中的所有函数都进行了this绑定。

2.  Component

        class Greeting extends React.Component{
            constructor(props){
                super(props);
                this.state = {count: props.initialCount};
                this.handleClick = this.handleClick.bind(this);
            }
            render(){
                return <h1>Hello,{this.props.name}</h1>
            }
        }

        Greeting.propTypes = {
            name: React.PropTypes.string   //React.PropTypes已经弃用了
        }

        Greeting.defaultProps = {
            name: 'Roy'
        }

        export default Greeting;

    用Component创建组件时，React并没有对内部的函数进行this绑定，如果需要，则要手动进行this绑定。

3. PureComponent

    (1)当组件的props或者state发生变化的时候：React会对组件当前的Props和State分别与nextProps和nextState进行比较，
    
    当发生变化时，就会对当前组件以及子组件进行重新渲染，否则就不渲染。

    (2)为了避免组件不必要的重新渲染，通过shouldComponentUpdate来优化性能。

    (3)大多数情况下，使用PureComponent来简化代码，提高性能。

    (4)但是PureComponent的自动添加的shouldConponentUpdate函数，只是对props和state进行浅比较（shadow comparison）。

    当props或者state本身是嵌套对象或数组时，浅比较并不能得到预期的结果，
    
    这会导致实际的props和state发生了变化，但是组件却没有更新的问题。

    (5)避免第(4)点的情况，就是避免使用可变对象作为props和state，取而代之的是每次返回一个全新的对象。
    
    比如：

            handleClick(){
                this.setState(prevState => ({
                    words: prevState.words.concat(['mark'])
                }))
            }
    
    (6)可以考虑使用immutable.js来创建不可变对象，通过它来简化对象比较，提高性能。

4. Stateless Functional Component

    当组件本身只是用来展示，所有数据都是用过props传入的时候，可以使用Stateless Functional Component来快速创建组件。

        import React from 'react';
        const Button = ({
            day,increment
        }) => {
            return (
                <div>
                    <button onClick={increment}>Today is {day}</button>
                </div>
            )
        }

        Button.propTypes = {
            day: PropTypes.string.isRequired,
            increment: PropTypes.func.isRequired
        }

    这种组件，没有自身的状态，相同的props输入，必然会获得完全相同的组件展示。


#### 【2】对比
1. createClass vs Component

    都是用来创建组件的，一个是ES5语法，一个是ES6语法。

2. PureComponent  vs Component

    PureComponent已经定义好了shouldComponentUpdate，而Component需要显示定义。

3. Component vs Stateless Functional Component

        A.Component包含内部state，而后者所有数据都来自props，没有内部state；

        B.Component包含的一些生命周期函数，后面都没有。
          因为后者没有shouleComponentUpdate，所以也无法控制组件的渲染，
          也就是说，只要收到新的props，后者就会重渲染。

        C.后者不支持Refs


#### 【3】选哪个
1. createClass 
    
    除非对es6语法一窍不通，否则不建议使用这种方式。

2. Stateless Functional Component 

    对于不需要内部状态，且用不到生命周期函数的组件，可以使用这种方式。

    比如展示性的列表组件，可以将列表项定义为这种方式。

3. PureComponent/Component

    对于拥有内部状态，使用生命周期的函数组件，我们可以使用两者之一。

    推荐使用PureComponent，因为它提供了更好的性能，同时强制你使用不可变的对象。



### 三、React PureComponent 使用指南
>http://www.wulv.site/2017-05-31/react-purecomponent.html   【重要】

#### 【1】为什么使用
PureComponent是取代其前身PureRenderMixin，可以减少不必要的render操作的次数，

从而提高性能，而且可以少些shouleComponentUpdate函数，节省了点代码。


#### 【2】原理
(1)当组件更新时，如果组件的props和state都没发生改变，render方法就不会触发，

省去了Virtual DOM的生成和对比过程，达到提升性能的目的。

(2)具体就是React自动帮我们做了一层浅比较：

    if(this._compositeType === Composites.PureClass){
        shouldUpdate = !shallowEqual(prevProps,nextProps) || !shallowEqual(inst.state,nextState)
    }

(3)shallowEqual的功能

    会比较object.keys(state|props)的长度是否一致，每一个key是否两者都有，并且是都是一个引用，
    也就是只比较了第一层的值，确实很浅，所以深层的嵌套数据是对比不出来的。


#### 【3】使用指南
1. 易变数据不能使用一个引用

2. 不变数据使用一个引用

3. 复杂状态与简单状态不要共用一个组件

4. 与shouldComponentUpdate共存

        如果PureComponent里有shouldComponentUpdate函数的话，
        直接使用shouldComponentUpdate的结果作为是否更新的依据，
        没有shouldCompoonentUpdate函数的话，才会去判断是不是PureComponent，是的话再去做shallowEqual浅比较。

5. 老版本兼容写法

        import React { PureComponent, Component } from 'react';
        class Foo extends (PureComponent || Component) {
            //...
        }

#### 【4】总结
    PureComponent 真正起作用的，只是在一些纯展示组件上，复杂组件用了也没关系，
    反正 shallowEqual 那一关就过不了，不过记得 props 和 state 不能使用同一个引用哦。


    

