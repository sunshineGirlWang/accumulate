### js获取滚动条距离浏览器顶部,底部的高度

#### 参考资料
>https://blog.csdn.net/alex8046/article/details/52691038

#### 摘要
一、获取窗口滚动条高度

    function getScrollTop(){    
        var scrollTop=0;    
        if(document.documentElement&&document.documentElement.scrollTop){    
            scrollTop=document.documentElement.scrollTop;    
        }else if(document.body){    
            scrollTop=document.body.scrollTop;    
        }    
        return scrollTop;    
    } 

#### 总结
1. 在使用position: fixed时，需要计算元素的位置。

    这时可以借助getScrollTop()方法来获取窗口滚动条高度，从而计算元素的top值。