## 一、参考资料
1. mobx中文文档

    http://cn.mobx.js.org/
2. mobx todolist

    https://codesandbox.io/s/2vmzpM0wK
3. mobx 在ReactJS项目中的运用

    https://blog.csdn.net/u012125579/article/details/69400169

## 二、入门
任何源自应用状态的东西都应该自动地获得。
1.与react的关系
    react提供了优化UI渲染的机制，这种机制就是通过使用虚拟DOM来减少昂贵的DOM变化的数量。mobx提供了优化应用状态与react组件同步的机制，这种机制就是使用响应式虚拟依赖状态图标，它只有在真正需要的时候才更新并且永远保持是最新的。
    
2.核心概念
    （1）Observable state（可观察的状态）
        通过使用@observable装饰器来给现有的数据结构（如对象、数组和类实例）添加可观察的功能。
        注：@observable是es.next的写法，在es5中，使用extendObservable()来实现。

    （2）Computed values（计算值）
        定义在相关数据发生变化时自动更新的值，可通过@computed 装饰器或者利用(extend)Observable时调用的getter/setter函数来进行使用。

    （3）Reactions（反应）
        reactions在响应式编程和命令式编程之间建立沟通的桥梁。reactions和计算值很像，但它不是产生一个新的值，而是会产生一些副作用，比如打印到控制台、网络请求、递增地更新react组件数以修补dom等。
    
    （4）Actions(动作)
        状态应该以某种状态来更新。
3.
4.