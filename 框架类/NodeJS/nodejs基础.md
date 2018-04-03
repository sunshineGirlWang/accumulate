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


