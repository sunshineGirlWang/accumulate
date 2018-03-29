# 关于Cookie与Storage
### 一、简介

1、localStorage：HTML5新增的在浏览器端存储数据的方法。(没有时间限制的)

设置和获取localStorage的方法：

    A.设置： localStorage.name = 'wqq';

    B.获取： localStorage.name //wqq

2、sessionStorage: HTML5新增的在浏览器端存储数据的方法。（关闭窗口，存储的数据清空）

设置和获取sessionStorage的方法：

    A.设置： sessionStorage.name = 'wqq';

    B.获取： sessionStorage.name //wqq

3、cookie：

浏览器和服务器端都可以设置cookie，传统的用来存储数据的方法。

设置和获取方法见：

    A.设置：

        function setCookie(name,value) 
        { 
            var Days = 30; 
            var exp = new Date(); 
            exp.setTime(exp.getTime() + Days*24*60*60*1000); 
            document.cookie = name + "="+ escape (value) + ";expires=" + exp.toGMTString(); 
        }

    B.读取：

        function getCookie(name) 
        { 
            var arr,reg=new RegExp("(^| )"+name+"=([^;]*)(;|$)"); 
        　　 return (arr=document.cookie.match(reg))?unescape(arr[2]):null;
        }

    C.删除：

        function delCookie(name) 
        { 
            var exp = new Date(); 
            exp.setTime(exp.getTime() - 1); 
            var cval=getCookie(name); 
            if(cval!=null) 
                document.cookie= name + "="+cval+";expires="+exp.toGMTString(); 
        }

### 二、三者的关系和使用场景：

1、关系：

    A. cookie在浏览器和服务器端来回传递数据，而localStorage和sessionStorage不会自动把数据发送给服务器，仅会保存在本地。cookie会在浏览器请求头或者ajax请求头中发送cookie内容。

    B. cookie可以设置过期日期，sessionStorage是会话级的数据，浏览器窗口关闭即清楚，localStorage是永久性的数据，一旦赋值，不管多长时间这值都是存在的，除非手动清除。

    C. cookie的存储大小受限制，一般不超过4k，而localStorage和sessionStorage的存储大小一般不超过5M，大大提高了存储的体积。

    D. sessionStorage不跨窗口，在另外一个窗口打开sessionStorage就不存在了，它只在当前窗口有效，而cookie和localStorage都是跨窗口的，即使浏览器的窗口关闭，这两个值还是存在的。

2、使用场景：

    A. localStorage可以用来统计页面访问次数。
    B. sessionStorage可以用来统计当前页面元素的点击次数。
    C. cookie一般存储用户名密码相关信息，一般使用escape转义编码后存储。

### 三、相关功能的实现：

1、要求记录用户刚刚浏览的位置，而不是重新刷新页面到了页面顶部.
https://blog.csdn.net/oaa608868/article/details/53539954

实现思路：①页面滚动，将滚动位置存到session中 → ②再次进到页面中，到session中取出上次保存的浏览位置 → ③滚动到对应位置

