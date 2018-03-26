原文链接：https://segmentfault.com/a/1190000008724608

1、jQuery的链式调用原理：jQuery节点在调用api后都会返回节点自身。
    var Obj = {
        a: 1,
        func: function(){
            this.a += 1;
            return this;
        }
    }
    Obj.func().func();
    console.log(Obj.a);

2、实现加法操作add(1)(2)(3)
    function add(num){
        var count = num;
        var b = function(c){
            count += c;
            return b;
        }   
        b.valueOf = function(){
            return count;
        }
        return b;
    }
    var c = add(1)(2)(3);
    console.log(c);

3、实现add(1)(2)(3)的详细分析：
A.首先，在add方法内部，是通过私有的b方法实现的加法，而不是在add方法自身实现的（函数式编程）。
B.在返回b方法中，形成了对count的闭包，这样我们可以实现累加记和。同时，b方法每次执行时都返回了它自身，这就实现了链式。
C.b是Function，是Object的一种特殊形式，当使用类似console等操作时，会自动调用其valueOf方法。所以重写valueOf方法可以达到返回count的效果。

4、链式调用的优缺点
A.优点：节省代码量，代码看起来更优雅。
B.缺点：所有对象的方法返回的都是对象本身，也就是说没有返回值，所以这种方法不一定在任何环境下都适合。

5、实现aQuery().init().name()
aQuery.prototype = {
init: function(){
return this;
},
name: function(){
return this;
}
}

6、理解这个demo
function Print() {
    var print = function () {
        return {
            Print: function () {
                console.log("aaaaa")
            }
        }
    }
    print.Print = function () {
        return function () {
            console.log("bbbbb")
        }
    }
    return print;
}
Print().Print()();//bbbbb
Print()().Print();//aaaaa



