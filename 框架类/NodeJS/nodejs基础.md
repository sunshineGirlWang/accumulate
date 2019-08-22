## 一、入门
1、
        var http = require("http");

        function process_request(req,res){
            var body = 'Thanks for calling!\n';
            var content_length = body.length;
            res.writeHead(200,{
                'Content-Length': content_length,
                'Content-Type': 'text/plain'
            })
            res.end(body);
        }

        var s = http.createServer(process_request);
        s.listen(8080);

## 二、进一步了解JavaScript
2、数据类型

2.1 核心类型：number、boolean、string、object.

    undefined表示还没有赋值或者不存在；
    null表示没有值。
    可以通过typeof操作符查看任何数据的类型。

2.2 常量

    理论上支持const关键字，但标准实践中仍使用大写字母和变量声明。

2.3 number 类型

    (1)64位双精度浮点数，2的53次方位。

    (2)Infinity、-Infinity：正无穷大和负无穷大在JavaScript里都是合法的值，并且可以使用它们进行比较。
        var x = 10,y = 0;
        x/y == Infinity;//true

    (3)isNaN(parseInt('cat')); //true

    (4)判断一个给定的值是否是一个合法的有限数（不是Infinity、-Infinity或者NaN）：
        isFinite(10/5)//true

2.4  boolean类型

    (1)false、0、空字符串（""）、NaN、null以及undefined都等价于false。
    (2)其他值都等价于true。

2.5 string类型

    (1)若在js中尝试获取值为null或undefined的字符串的长度时，将会抛出错误。

    (2)将两个字符串组合在一起，可以使用+操作符。
    
    (3)如果将其他数据类型的数据混入到字符串中，js将尽可能将其他数据转换成字符串。

    (4)字符串函数：
        A.indexOf(): 在一个字符串中搜索另外一个字符串。
        B.substr()或splice(): 从一个字符串中截取一个子串。
            substr接收一个开始索引和一个需要截取的字符串长度；
            splice接收一个开始索引和一个结束索引。
        C.split(): 将字符串分割成子字符串并返回一个数组。
        D.trim(): 清除字符串前后的空白字符。
    
    (5)正则表达式：
         A.new RegExp()
         B.replace()
         C.search():接收一个正则表达式参数，并返回第一个匹配此正则表达式的子字符串的位置索引，如果匹配不存在，则返回-1

2.6 object类型

    (1)var o1 = {};

    (2)关于JSON（JavaScript Object Notation）:
        A.JSON中所有字符串都需要包含在双引号中。
        B.可以使用V8的JSON.parse和JSON.stringify函数来生成JSON数据。
            前者接收一个JSON字符串作为参数，并将其转换成一个对象（如果失败，则抛出一个错误）；
            后者接收一个对象作为参数，并返回一个JSON字符串表示。

    (3)如果尝试访问一个不存在的属性，并不会报错，而是会得到undefined这样的结果。

    (4)删除对象的某个属性，可以使用delete关键字；

    (5)
    var user = {
        "first_name": 'ice'
    }
    Object.keys(user).length   //获取对象的大小，结果为1

2.7 array类型

    (1)var arr = [];

    (2)typeof arr //'object'

    (3)Array.isArray(arr) //true

    (4)arr.length  //数组的长度

    (5)元素添加：
    A.js数组可以通过数字来进行索引的
        var arr1 = ['cat','dog'];

    B.还可以在数组的末尾添加新项
        arr1.push('rabbit');
        arr1[arr1.length] = 'monkey';

    C.删除元素
        delete  arr1[2];// 数据会被设置成undefined，但位置还在
        arr1.splice(1,1);//splice函数会接收删除项的起始索引和数目作为参数。该函数会返回被删除的数组项，并且原始数组已经被修改，这些项不再存在。

    D.实用函数
        push() 或 pop() //向数组的末尾添加或者删除元素
        unshift() 或 shift()  //在数组的头部插入或者删除元素
        join() //将数组拼接成字符串
        sort() //数组的排序
        forEach(function(value,key){......})  //数组的遍历

3、类型比较和转换

    (1)
        ==  相等运算符（判断两个操作数有没有相同的值）
        === 严格相等运算符（判断两个操作数有没有相同的值以及是否为相同的数据类型）

    (2)强烈建议在任何可能的情况下都只使用原始数据类型。
        虽然对象构造器在功能上和原始数据类型是一样的，但是在===和typeof操作符则会产生不同的结果。

4、函数

    (1)基本概念
        A.当函数被调用时，如果传入的参数不够，剩下的变量会被赋予undefined值。而如果传入的参数过多，则多余的参数会被简单地做无用处理。

        B.所有函数在函数体内都会有一个叫做arguments的预定义数组。它拥有函数调用时所有传入的实参，让我们可以对参数列表做额外的检查。

        C.缺少名字的函数通常叫做匿名函数（anonymous function），完全匿名的函数有一个缺点，就是在调试时，抛出的异常中不会告知出现异常的函数名称，会导致调试变得更加困难。
    
    (2)函数作用域
        A.每次调用函数，都会创建一个新的变量作用域。父作用域中声明的变量对该函数是可见的，但是，当函数退出后，该函数作用域中声明的变量就会失效。

        B.可以将作用域和匿名函数结合起来做一些快速或私有的工作。这样，当匿名函数退出后，里面的私有变量也会消失。
        
5、语言结构

    (1)支持三元运算符
    (2)支持位操作符：& | ~ ^
    (3)支持while、do...while、for、for...in
        var user = {
            first_name: 'nana',
            last_name: 'enheng'
        }
        for(key in user){
            console.log(key);
        }

6、类、原型和继承

    (1)JS中所有的类都是以函数的形式定义的。

    (2)默认情况下，所有的JS对象都有一个原型（prototype）对象，它是一种继承属性和方法的机制。

    (3)__proto__属性：它能够告诉JS声明的新类的基本原型应该是指定的类型，因此也就可以从指定的类进行扩展。
        function Shape(){    
        }
        Shape.prototype.X = 0;
        Shape.prototype.Y = 0;
        Shape.prototype.move = function(x,y){
            this.X = x;
            this.Y = y;
        }
        Shape.prototype.distance_from_origin = function(){
            return Math.sqrt(this.X * this.X + this.Y * this.Y);
        }
        Shape.prototype.area = function(){
            throw new Error("I don't have a form yet");
        }

        var s = new Shape();
        s.move(10,10);
        console.log(s.distance_from_origin());

        function Square(){       
        }
        Square.prototype = new Shape();
        Square.prototype.__proto__ = Shape.prototype;
        Square.prototype.Width = 0;
        Square.prototype.area = function(){
            return this.Width * this.Width;
        }

        var sq =  new Square();
        sq.move(-5,-5);
        sq.Width = 5;
        console.log(sq.area());
        console.log(sq.distance_from_origin());


7、错误和异常

    (1)JS中，通常使用Error对象和一条信息来标识一个错误。

    (2)可以通过try...catch语句块来捕捉错误。

8、几个重要的Node.js全局对象

    (1)global对象
        任何附加到该对象上的东西在node应用中的任何地方都是可见的。

    (2)console对象
        A.warn(msg)：与log类似的函数，但打印的是标准错误stderr。

        B.time(label)和timeEnd(label)：第一个函数被调用时会标识一个时间戳，而当第二个函数被调用时，会打印出从time函数被调用后中间经过的时间。

        C.assert(cond,message):如果cond等价于false，则抛出一个AssertionFailure异常。

    (3)process对象
        process对象包含许多信息和方法。
        exit方法是终止Node.js程序的方式之一；
        env函数会返回一个对象，包含了当前用户的环境变量；
        cwd返回应用程序当前的工作目录。

## 三、异步编程
    非阻塞IO、异步编程
9、最大化利用CPU的计算能力和可用内存以减少资源浪费。

10、异步函数如何工作的关键方法

    (1)检查和验证参数
    (2)通知Node.js核心去排队调用相应的函数，并在返回结果的时候通知（调用）回调函数
    (3)返回给调用者
  
11、错误处理和异步函数

    (1)
        do_something(param1,param2,...,paramN,function(err,results){...})
        参数err的值一般会是：
        A.null，表明操作成功，并且会有一个返回值（如果有需要的话）。
        B.一个Error对象的实例。

    (2)在回调函数中处理错误的两种不同的代码风格
        A.one style
            function(err,handle){
                if(err){
                    console.log("ERROR:"+ err.code + "("+ err.message+")");
                    return;
                }
                //if success,continue working here
            }

        B.other style
            function(err,handle){
                if(err){
                    console.log("ERROR:"+ err.code + "("+ err.message+")");
                }else{
                    //if success,continue working here
                }
            }

12、我是谁————如何维护本体

    由于函数作用域是通过闭包保留的，所以self变量会被一直保持着，即使回调函数是被Node.js异步执行的。你可以选择一个喜欢的命名，然后一直用下去，以保持一致性。

13、保持优雅————学会放弃控制权

    (1)Node运行在单线程中，使用事件轮询来调用外部函数和服务。它将回调函数插入事件队列中来等待响应，并且尽快执行回调函数。

    (2)不适合的应用场景：作为计算服务器

    (3)适合的应用场景：适合一些常见的网络应用服务，比如那些需要大量I/O或者需要向其他服务请求的任务。

    (4)如果只是偶尔执行高强度计算的任务，那么可以利用全局对象process中的nextTick方法。（这个方法就好像在跟系统说：我放弃执行控制权，你可以在空闲的时候执行我提供给你的函数。）

14、同步函数调用

    var fs = require('fs');
    var handle = fs.openSync('info.txt','r');
    var buf = new Buffer(100000);
    var read = fs.readSync(handle,buf,0,10000,null);
    console.log(buf.toString('utf8',0,read));
    fs.closeSync(handle);

## 四、编写简单应用
15、第一个JSON服务器

    var http = require('http');
    function handle_incoming_request(req,res){
        console.log("INCOMING REQUEST:" + req.method + "" + req.url);
        res.writeHead(200,{"Content-Type": "application/json"});
        res.end(JSON.stringify({error: null}) + "\n");
    }
    var s = http.createServer(handle_incoming_request);
    s.listen(8080);

16、常用的状态码

    (1)200 OK
        ——一切正常。
    (2)301 Moved Permanently
        ——请求的URL已被移走，客户端应该重新请求响应中指定的URL。
    (3)400 Bad Request
        ——客户端请求的格式是无效的，需要修复。
    (4)401 Unauthorized
        ——客户端没有权限查看请求的资源，它应该先验证请求后再试。
    (5)403 Forbidden
        ——无论出于何种原因，服务器拒绝处理这个请求。
        这和401不一样，401中客户端在验证通过后可以重试。
    (6)404 Not Found
        ——客户端请求的资源不存在。
    (7)500 Internal Server Error
        ——某些情况发生导致服务器无法处理请求。
        通常这个错误代表代码已经处于某种不一致的状态或出现bug，需要开发人员关注。
    (8)503 Service Unavailable
        ——这表明某种运行时错误，例如内存暂时不足或网络资源出现问题。
        它和500一样都是致命的错误，但它也表明客户端可以过段时间后再次尝试。

17、修改内容：POST数据

    程序为了获取POST数据，会使用一种叫做数据流的Node特性。
    当使用Node的异步非阻塞特性时，数据流是传输大量数据的最佳方式。

    (1)设置HTTP方法参数为POST（或者PUT）
    (2)为传入的数据设置Content-Type
    (3)开始发送数据

## 五、模块化
18、编写简单模块

    (1)模块是Node.js对常用功能进行分组的方式。

    (2)exports对象是一个特殊的对象，在每个我们创建的文件中由Node模块系统创建，当引入这个模块时，会作为require函数的值返回。它被封装在每个模块的module对象中，用来暴露函数、变量或者类。

    (3)工厂模式（通常使用这种方法）
        优点：模块可以通过exports对象暴露其他的函数和类。
        function ABC(params){
            this.varA = ...;
            this.varB = ...;
            this.functionA = function(){
                ...
            }
        }

        exports.create_ABC = function(params){
            return new ABC(params);
        }

    (4)构造函数模式（知道有这种模式即可）
        优点：模块真正唯一暴露的是一个类的构造函数。
        缺点：不能让模块暴露更多的东西。
         function ABC(params){
            this.varA = ...;
            this.varB = ...;
            this.functionA = function(var1,var2){
                console.log(var1 + " " + var2);
            }
        }

        module.expoets = ABC;

        为了使用该模块，需要修改代码：
        var ABCClass = require('./conmod2');
        var obj = new ABCClass();
        obj.function(1,2);

19、npm:Node包管理器

    npm install  XXX //安装
    npm search XXX  //打印出所有符合条件的模块的名字和描述
    npm ls  //查看某个项目当前使用的所有模块清单
    npm update XXX  //更新

20、使用模块

    (1)为了能够引用模块里的函数和类，需要将结果（被加载的模块的exports对象）赋值给一个变量：
        var http = require('http');

    (2)引入的模块对于引入它们的模块是私有的。

    (3)查找模块
        A.如果请求的是内置模块————例如http或者fs————Node会直接使用这些模块。

        B.如果require函数的模块名以路径符开始（如./、../或者/），那么Node会在指定的目录中查找模块并尝试去加载它。
          如果没有在模块名中指定.js扩展名，Node会首先查找基于同名文件夹的模块。
          如果没有找到，它会添加扩展名.js、.json和.node，
            并依次尝试加载这些类型的模块（带有.node扩展名的模块会被编译成附加项目）。

        C.如果模块名开始时没有路径符,Node会在当前文件夹的node_modules/子文件夹下查找模块。
          如果找到，则加载该模块；
            否则，Node会以自己的方式在当前位置的路径树下搜索node_modules/文件夹。
          如果依然失败，它会在一些默认地址下进行搜索，例如/usr/lib和/usr/local/lib文件夹。

        D.如果在以上任何一个位置都没有找到模块，则抛出错误。

    (4)模块缓存
        A.当模块从指定的文件或者目录上加载之后，Node.js会将它缓存。
        B.之后所有的require调用将会从相同的地址加载相同的模块————而这些模块已经初始化或者做过其他工作。

    (5)循环
        考虑下面的情况：
        ■　a.js引入b.js。
        ■　b.js引入a.js。
        ■　main.js引入a.js。

        很显然，我们会发现上面的模块中存在一个循环。
        当Node探测到一个尚未初始化的返回模块时，它会阻止循环发生。
        在上面的示例中，会发生下面的事情：
        ■　main.js被加载，运行引入a.js的代码。
        ■　a.js被加载，运行引入b.js的代码。
        ■　b.js被加载，运行引入a.js的代码。
        ■　Node探测到循环并返回一个指向a.js的对象，
        但不会再执行其他代码——在这一刻，a.js的加载和初始化并没有完成。
        ■　b.js、a.js和main.js都完成初始化（按顺序），
        然后b.js和a.js的引用都有效和可用。

21、编写模块
    (1)基本格式如下：
        A.创建一个文件夹来存放模块内容。
        B.添加名为package.json的文件到文件夹中。
          文件至少包含当前模块的名字和一个主要的JavaScript文件————用来一开始加载该模块。
        C.如果Node没有找到package.json文件或者没有指定主JavaScript文件，
          那它就会查找index.js（或者是编译后的附加模块index.node）。

    (2)使用模块进行开发
        package.json文件，如果增加了private属性，则表示：npm不能将现在还不想发布的模块发布到外部的npm源中。
        {
            "name": "album-manager",
            "version": "1.0.0",
            "main": "./lib/albums.js",
            "private": true
        }
        
22、发布模块
    (1)如果想要把模块共享，就需要使用npm publish发布到官方的npm模块注册中心。

        A.删除package.json文件的"private":true。

        B.在npm注册服务器上使用npm adduser命令创建一个账户。

        C.可以选择向package.json文件中添加更多字段（npm help json）。

        D.在模块的目录上执行 npm publish ，把模块发布到npm。

    (2)移除模块：npm unpublish

23、async的npm模块
    async.waterfall
    async.series
    async.parallel
    async.auto

24、使用Stream处理静态内容
    

25、

26、