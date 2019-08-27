### Dva概念
#### 数据流向 
    数据的改变发生通常是通过用户交互行为或者浏览器行为触发的。
    当此类行为会改变数据的时候可以通过dispatch发起一个Action，
    如果是同步行为，会直接通过Reducers改变State；
    如果是异步行为，会先触发Effects，然后流向Reducers，最终改变State。

#### Models
1、State
    type State = any

    State表示Model的状态数据，通常表现为一个javascript对象；
    操作的时候每次都要当做不可变数据（immutable data）来对待，保证每次都是全新对象，没有引用关系。
    这样能保证State的独立性，便于测试和追踪变化。

    const app = dva();
    console.log(app._store); // 顶部的 state 数据

2、Action
    type AsyncAction = any

    Action是一个普通的javascript对象，它是改变State的唯一途径。
    无论是从UI事件、网络回调，还是WebSocket等数据源所获得的数据，
    最终都会通过dispatch函数调用一个action，从而改变对应的数据。

    action必须带有type属性指明具体的行为，其他字段可以自定义，
    如果要发起一个action需要使用dispatch函数；
    需要注意的是dispatch是在组件connect Models以后，通过props传入的。

    dispatch({
        type: 'add',
    })

3、dispatch函数
    type dispatch = (a:Action) => Action

    dispatch函数是一个用于触发action的函数。

    在dva中，connect Model的组件通过props可以访问到dispatch，
    可以调用Model中的Reducer或者Effects。

    常见形式如：
    dispatch({
        type: 'user/add', //如果在model外调用，需要添加namespace
        payload: {} //需要传递的信息
    })

4、Reducer
    type Reducer<S,A> = (state: S,action: A) => S

    Reducer函数接收两个参数: 之前已经累积运算的结果和当前要被累积的值，
    返回的是一个新的累积结果。
    该函数把一个集合归并成一个单值。

    reducer的特点：1、纯函数（同样的输入必然得到同样的结果）
    2、每一次计算都应该使用immutable data。

5、Effect 副作用
    Effect被称为副作用。因为这会使得函数变得不纯，同样的输入不一定获得同样的输出。
    
    最常见的就是异步操作。

    在dva中，为了控制effect的操作，底层引入了redux-sagas做异步流程控制。
    采用generator的相关概念，将异步转换成同步的写法，从而将effects转换成纯函数。

6、Subscriptions  订阅
    Subscriptions是一种从源获取数据的方法。
    用于订阅一个数据源，然后根据条件dispatch需要的action。
    
    数据源可以是当前的时间、服务器的websocket连接、keyboard输入、geolocation变化，history路由变化等等。

    import key from 'keymaster';
    ...
    app.model({
        namespace: 'count',
        subscriptions: {
            keyEvent({dispatch}) {
            key('⌘+up, ctrl+up', () => { dispatch({type:'add'}) });
            },
        }
    });

#### Router
    dva提供了router方法来控制路由，使用的是react-router。
    
    import { Router, Route } from 'dva/router';
    app.router(({history}) =>
        <Router history={history}>
            <Route path="/" component={HomePage} />
        </Router>
    );

#### Route Components
    在dva中，通常需要connect Model的组件都是 Route Components,组织在 /routes/ 目录下，
    而/components/ 目录下则是纯组件。


### 入门课
#### dva是什么
    dva = React-Router + Redux + Redux-saga

#### dva应用的最简结构
    
    import dva from 'dva';
    const App = () => <div>Hello dva</div>;

    //创建应用
    const app = dva();
    //注册视图
    app.router(() => <App />);
    //启动应用
    app.start('#root');

#### 核心概念
    1、State：一个对象，保存整个应用状态

    2、View：React组件构成的视图层

    3、Action：一个对象，描述UI层事件。

    4、connect方法：一个函数，绑定State到View

    5、dispatch方法：一个函数，发送Action到State

#### State和View
    State是储存数据的地方，收到Action以后，会更新数据。
    View就是React组件构成的UI层，从State取数据后，渲染成HTML代码。
    只要State有变化，View就会自动更新。
   
#### connect方法
    connect方法返回的是一个React组件，通常称为容器组件。

    connect方法传入的第一个参数是 mapStateToProps 函数，
    mapStateToProps函数会返回一个对象，用于建立State到Props的映射关系。

    import { connect } from 'dva';

    function mapStateToProps(state){
        return { todos: state.todos };
    }
    connect(mapStateToProps)(App);

#### dispatch方法
    dispatch方法从哪里来的呢？
    被connect的Component会自动在props中拥有dispatch方法。

    dispatch({
        type: 'click-submit-button',
        payload: this.form.data
    })

#### dva应用的最简结构（带model）

    // 创建应用
    const app = dva();

    // 注册 Model
    app.model({
        namespace: 'count',
        state: 0,
        reducers: {
            add(state) { return state + 1 },
        },
        effects: {
            *addAfter1Second(action, { call, put }) {
            yield call(delay, 1000);
            yield put({ type: 'add' });
            },
        },
    });

    // 注册视图
    app.router(() => <ConnectedApp />);

    // 启动应用
    app.start('#root');

#### app.model
    dva提供app.model这个对象，所有的应用逻辑都定义在它上面。
    
    const app = dva();

    //新增这一行
    app.model({ /**/ });

    app.router(() => <App />);
    app.start("#root");

#### Model对象的属性
    1、namespace： 当前Model的名称。整个应用的State，由多个小的Model的State 以nmespace为key合成。

    2、state：该Model当前的状态。数据保存在这里，直接决定了视图层的输出。

    3、reducers：Action处理器，处理同步动作，用来算出最新的State。

    4、effects：Action处理器，处理异步动作。

#### Effect
    Action处理器，处理异步动作，基于redux-saga实现。
    
    根据函数式编程，计算以外的操作都属于Effect。最典型的就是I/O操作，数据库读写。

    Effect是一个Generator函数，内部使用yield关键字，标识每一步的操作。（不管是异步还是同步）

#### call和put
    dva提供多个effect函数内部的处理函数，比较常用的是call和put。

    1、call：执行异步函数。

    2、put：发出一个Action，类似于dispatch。

### 使用dva开发复杂SPA
#### 动态加载model
    通常使用webpack的require.ensure来做代码模块的懒加载。
    
    function RouterConfig({ history, app }) {
        const routes = [
            {
                path: '/',
                name: 'IndexPage',
                getComponent(nextState, cb) {
                    require.ensure([], (require) => {
                    registerModel(app, require('./models/dashboard'));
                    cb(null, require('./routes/IndexPage'));
                    });
                },
            },
            {
                path: '/users',
                name: 'UsersPage',
                getComponent(nextState, cb) {
                    require.ensure([], (require) => {
                    registerModel(app, require('./models/users'));
                    cb(null, require('./routes/Users'));
                    });
                },
            },
        ];

        return <Router history={history} routes={routes} />;
    }

#### 使用model共享全局信息
    
