/**
@Author:sunshineGirlWang O(∩_∩)O 
@AddTime:20180327
@参考资料：
    https://www.zhihu.com/question/35436669/answer/62753889
    https://segmentfault.com/a/1190000010211622
    https://blog.csdn.net/ibelieve1974/article/details/55220015
**/

一、mock的基础解释
    1、目的：可以在不依赖后端环境的情况下，进行前端开发，帮助编写单元测试。
    2、实现功能：
        A.能渲染模板
        B.实现请求路由映射
        C.数据接口代理到生产或者测试环境

二、mock的用法
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
        模板规则：'name|rule':value
        其中，name:属性名；
              rule:属性规则；
              value：属性值。
        

        3.2

        3.3

        3.4

        3.5

三、mock的原理


