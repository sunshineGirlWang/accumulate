@Author:sunshineGirlWang O(∩_∩)O 

@AddTime:20180327

@参考资料：
    https://www.zhihu.com/question/35436669/answer/62753889
    https://segmentfault.com/a/1190000010211622
    https://blog.csdn.net/ibelieve1974/article/details/55220015

### 一、mock的基础解释
1、目的：可以在不依赖后端环境的情况下，进行前端开发，帮助编写单元测试。

2、实现功能：
    A.能渲染模板
    B.实现请求路由映射
    C.数据接口代理到生产或者测试环境

### 二、mock的用法
1、安装

    npm install mockjs

2、使用mock

        var Mock = require('mockjs');
        var mock = Mock.mock({
            ......
        })

3、语法

 mock的语法规范包含两层规范：数据模板（DTD）和数据占位符（DPD）。

3.1 数据模板DTD

    A.模板规则：'name|rule':value
        其中，name:属性名； rule:属性规则；value：属性值。
            属性名和规则之间用|隔开，规则是可以选的。

    B.7个rule
        'name|min-max':value
        'name|count':value
        'name|min-max.dmin-dmax':value
        'name|min-max.dcount':value
        'name|count.dmin-dmax':value
        'name|count.dcount':value
        'name|+step':value

        注：（1）生成规则需要根据属性值的类型才能确定
            （2）属性值可以含有@占位符
            （3）属性值可以指定最终值的初始值和类型

    C.属性值是String
        var data = Mock.mock({
            'name1|1-3' : 'a', //重复生成1到3个a
            'name2|2' : 'b' //生成bb
        })
    
    D.属性值是Number
        var data = Mock.mock({
            'name1|+1' : 4,  //生成4,如果循环每次加1
            'name2|1-7' : 2, //生成一个数字，1到7之间
            'name3|1-4.5-8' : 1  //生成一个小数，整数部分1到4，小数部分5到8位（！！！表示小数点之后的位数范围）
        })

    E.属性值是Boolean
        var data = Mock.mock({
            'name|1': true, //生成一个布尔值，各一半
            'name1|1-3': true  // 1/4是true，3/4是false
        })

    F.属性值是Object
        var obj = {
            a:1,
            b:2,
            c:3,
            d:4
        }
        var data = Mock.mock({
            'name|1-3': obj,
            'name|2': obj
        })

    G.属性值是Array
        var arr = [1,2,3];
        var data = Mock.mock({
            'name1|1': arr, //从数组里随机取出1个值
            'name2|2': arr, //数组重复count次，这里count为2
            'name3|1-3': arr  //数组重复1到3次
        })

    H.属性值是Function
    I.属性值是RegExp

3.2

3.3

3.4

3.5

### 三、mock的原理


