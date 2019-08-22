### 简介 
#### 需要安装的依赖：
    redux react-redux redux-devtools 

#### 要点
    1、应用中所有的state都以一个对象树的形式存储在一个单一的store中。
    2、唯一改变state的办法是触发一个描述发生什么的对象，即action。
    3、为了描述action如何改变state树，需要编写reducers。

    注：redux只有一个store，一个根基的reduce函数（reducer）。

#### demo:

    import { createStore } from 'redux'

    /**
    * 这是一个 reducer，形式为 (state, action) => state 的纯函数。
    * 描述了 action 如何把 state 转变成下一个 state。
    *
    * state 的形式取决于你，可以是基本类型、数组、对象、
    * 甚至是 Immutable.js 生成的数据结构。惟一的要点是
    * 当 state 变化时需要返回全新的对象，而不是修改传入的参数。
    *
    * 下面例子使用 `switch` 语句和字符串来做判断，但你可以写帮助类(helper)
    * 根据不同的约定（如方法映射）来判断，只要适用你的项目即可。
    */
    function counter(state = 0, action) {
    switch (action.type) {
        case 'INCREMENT':
        return state + 1
        case 'DECREMENT':
        return state - 1
        default:
        return state
    }
    }

    // 创建 Redux store 来存放应用的状态。
    // API 是 { subscribe, dispatch, getState }。订阅、调度、获取状态
    let store = createStore(counter)

    // 可以手动订阅更新，也可以事件绑定到视图层。
    store.subscribe(() => console.log(store.getState()))

    // 改变内部 state 惟一方法是 dispatch 一个 action。
    // action 可以被序列化，用日记记录和储存下来，后期还可以以回放的方式执行
    store.dispatch({ type: 'INCREMENT' })
    // 1
    store.dispatch({ type: 'INCREMENT' })
    // 2
    store.dispatch({ type: 'DECREMENT' })
    // 1

#### 核心概念
    强制使用action来描述所有变化带来的好处是可以清晰地知道应用中到底发生了什么。

#### 三大原则
1、单一数据源
    整个应用的state被储存在一棵object tree中，并且这个object tree只存在于唯一一个store中。

2、state是只读的
    唯一改变state的方法就是触发action，action是一个用于描述已发生时间的普通对象。

3、使用纯函数来执行修改
    为了描述action如何改变state tree，需要编写reducers。

#### 生态系统
redux-thunk：
    调度（dispatch）函数，调用并将 dispatch 和 getState 作为参数。 这充当了 AJAX 调用和其他异步行为的方法。
    适用：入门、简单的异步和复杂的同步逻辑。

redux-saga：
    使用同步查找生成器函数处理异步逻辑。 Sagas 返回 effect 的描述， 这些 effect 由 saga 中间件执行，并且像 JS 应用程序的“后台线程”。
    适用：复杂的异步逻辑，解耦的工作流程。

redux-observable：
    使用称为 “epics” 的 RxJS 可观察链处理异步逻辑。 撰写和取消异步操作以创建副作用等。
    适用:复杂的异步逻辑，解耦的工作流程。


### 基础
#### Action
1、action是把数据从应用传到store的有效载荷。是store数据的唯一来源。

2、一般通过store.dispatch()将action传到store。

3、action内必须使用一个字符串类型的type字段来表示将要执行的动作。

4、多数情况下，type会被定义成字符串常量。

5、建议使用单独的模块或文件来存放action。

6、应尽量减少在action中传递的数据。

7、action创建函数 就是生成action的方法。在redux中的action创建函数只是简单的返回一个action。

8、demo
    //定义一个action创建函数
    function addTodo(text) {
        return {
            type: ADD_TODO,
            text
        }
    }

    //method1 
    dispatch(addTodo(text))

    //method2
    const boundAddTodo = text => dispatch(addTodo(text))
    boundAddTodo(text);

#### Reducer
1、reducers指定了应用状态的变化如何响应actions并发送到store的，
记住actions只是描述了有事情发生了这一事实，并没有描述应用如何更新state。

2、reducer是一个纯函数，接收旧的state和action，返回新的state。

    //reducer
    (previousState,action) => newState

3、永远不要在recuder里做这些操作：
（1）修改传入参数；
（2）执行有副作用的操作，如API请求和路由跳转；
（3）调用非纯函数，如Date.now()，Math.random()。

4、只要传入参数相同，返回计算得到的下一个state就一定相同。
没有特殊情况、没有副作用、没有API请求，没有变量修改、单纯执行计算。

5、demo

    import { VisibilityFilters } from './actions'

    const initialState = {
        visibilityFilter: VisibilityFilters.SHOW_ALL,
        todos: []
    }

    function todoApp(state = initialState, action) {
        // 这里暂不处理任何 action，
        // 仅返回传入的 state。
        return state
    }

    function todoApp(state = initialState, action) {
        switch (action.type) {
            case SET_VISIBILITY_FILTER:
            //使用Object.assign()新建了一个副本，必须把第一个参数设置为空对象，否则会改变第一个参数的值。
            //也可写成
            // return {...state,...{
            //    visibilityFilter: action.filter
            // }}
                return Object.assign({}, state, {
                    visibilityFilter: action.filter
                })
            default:
                return state
        }
    }

6、不要修改state。
    使用Object.assign() 或 扩展运算符来实现。

7、在default情况下，返回旧的state。遇到未知的action时，一定要返回旧的state。

8、每个reducer只负责管理全局state中它负责的一部分。每个reducer的state参数都不同，分别对应它管理的那部分state数据。

9、redux中的combineReducers()可以生成一个函数来调用一系列的reducer，
每个reducer根据他们的key来筛选出state中的一部分数据并处理，
然后这个生成的函数再将所有的reducers的结果合并成一个大的对象。

10、combineReducers 接收一个对象，
可以把所有顶级的 reducer 放到一个独立的文件中，通过 export 暴露出每个 reducer 函数，
然后使用 import * as reducers 得到一个以它们名字作为 key 的 object：

    import { combineReducers } from 'redux'
    import * as reducers from './reducers'

    const todoApp = combineReducers(reducers)

#### Store
1、职责：
    （1）维持应用的state；
    （2）提供 getState() 方法获取state；
    （3）提供 dispatch(action) 方法更新state；
    （4）提供 subscribe(listener) 注册监听器；
    （5）提供 subscribe(listener) 返回的函数注销监听器。

2、redux的createStore():创建一个Redux store来以存放应用中所有的state，应用中应有且仅有一个store。
第2个参数是可选的，用于设置state初始状态。

    import { createStore } from 'redux'
    import todoApp from './reducers'
    let store = createStore(todoApp)

3、demo

    import {
        addTodo,
        toggleTodo,
        setVisibilityFilter,
        VisibilityFilters
    } from './actions'

    // 打印初始状态
    console.log(store.getState())

    // 每次 state 更新时，打印日志
    // 注意 subscribe() 返回一个函数用来注销监听器
    const unsubscribe = store.subscribe(() => console.log(store.getState()))

    // 发起一系列 action
    store.dispatch(addTodo('Learn about actions'))
    store.dispatch(addTodo('Learn about reducers'))
    store.dispatch(addTodo('Learn about store'))
    store.dispatch(toggleTodo(0))
    store.dispatch(toggleTodo(1))
    store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))

    // 停止监听 state 更新
    unsubscribe()

#### 数据流
redux架构的设计核心是**严格的单向数据流**
redux应用中数据的生命周期遵循以下4个步骤：
1、调用 store.dispatch(action)
2、redux store 调用传入的reducer函数
3、根reducer应该把多个子reducer输出合并成一个单一的state树
4、redux store保存了根reducer返回的完整state树

#### 搭配React
redux与React没有关系，redux支持React、Angular、Ember、jQuery甚至纯JavaScript。

npm install --save react-redux

#### demo
1、分为actions、reducers、components、containers这几个文件夹。

2、actions包含index.js，定义了很多常量的常量。

3、reducers包含了index.js和定义各个子reducer的js文件。

index.js中使用redux的combineReducers()方法，将各个子reducer聚合在一起。

4、components中的文件均是业务组件。

5、containers中的文件均使用了react-redux的connect()方法，用于连接react组件与redux store。

7、App.js中最顶层使用了react-redux的<Provider></Provider>。

#### 关于react-redux
1、仅有2个API，Provider和connect。

2、Provider提供的是一个顶层容器的作用，实现store的上下文传递。

3、一个基础的connect方法如下：

    connect(mapStateToProps, mapDispatchToProps, mergeProps, options = {}) 

### 高级
#### 异步action
#### 异步数据流
#### middleware
#### 搭配 react router
#### 搭配typescript

### 技巧