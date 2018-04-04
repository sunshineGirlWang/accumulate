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

