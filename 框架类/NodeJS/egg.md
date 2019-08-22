### 一、参考文档
1、egg的官方文档
    https://eggjs.org/zh-cn/

2、react+egg服务端渲染开发指南
    https://segmentfault.com/a/1190000011719737

### 二、新手指南
1、设计原则
    A.高扩展性，一个插件只做一件事
    B.约定优于配置，按照一套统一的约定进行应用开发
    （使用Loader可以让框架根据不同环境定义默认配置，还可以覆盖Egg的默认约定）

2、特性
    A.提供基于Egg定制上层框架的能力
    B.高度可扩展的插件机制
    C.内置多进程管理
    D.基于Koa开发，性能优异
    E.框架稳定，测试覆盖率高
    F.渐进式开发

3、Egg.js与Koa
(1)异步编程模型
    官方API支持的都是callback形式的异步编程模型，会引起的问题：
    A.callback hell:callback嵌套问题。
    B.release zalgo:异步程序中可能同步调用callback返回数据，带来不一致性。

(2)async function
    语法糖，在async function中，可以通过await关键字来等待一个Promise被resolve（或者reject，此时会抛出异常）。
    const fn = async function(){
        const user = await getUser();
        const posts = await fetchPosts(user.id);
        return {user,posts};
    }
    fn().then(res => console.log(res))
    .catch(err => console.log(err.stack));

(3)Koa主要特点
    A.Middleware是洋葱圈模型，
      所有的请求经过一个中间件的时候都会执行两次，koa的模型可以非常方便的实现后置处理逻辑。

    B.增加了一个Context的对象，作为这次请求的上下文对象。
      Context上也挂载了Request和Response两个对象。
      这两个对象提供了：
      i) get request.query
      ii) get request.hostname
      iii) set response.body
      iv)  set resopnse.status
      
    C.异步处理
      通过同步方式编写异步代码带来的最大的好处就是处理非常自然，
      使用try catch就可以将按照规范编写的代码中的所有错误都捕获到。
      async function oerror(ctx,next){
          try{
              await next();
          }catch(err){
              ctx.app.emit('error',err);
              ctx.body = 'server error';
              ctx.status = err.status || 500;
          }
      }
      只需要将这个中间件放在其他中间件之前，就可以捕获它们所有的同步或者异步代码中抛出的异常了。

(4)Egg继承于Koa
    Egg在Koa的基础上做了一些增强功能。
    A.扩展
        可通过定义 app/extend/{application,context,request,response}.js来扩展Koa中对应的四个对象的原型。

    B.插件
        一个插件可以包含：
        i) extend:扩展基础对象的上下文，提供各种工具类、属性。
        ii) middleware:增加一个或多个中间件，提供请求的前置、后置处理逻辑。
        iii) config:配置各个环境下插件自身的默认配置项。

4、快速入门




### 三、基础功能

### 四、核心功能

