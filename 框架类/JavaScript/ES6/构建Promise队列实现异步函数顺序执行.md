### 构建Promise队列实现异步函数顺序执行

#### 场景：
1. 有a、b、c三个异步任务，要求必须先执行a，再执行b，最后执行c

2. 且下一次任务必须要拿到上一次任务执行的结果，才能做操作

#### 代码实现：

        const timeout = ms => new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve();
            }, ms);
        });

        const ajax1 = () => timeout(2000).then(() => {
            console.log('1-1');
            return 1;
        });

        const ajax2 = () => timeout(1000).then(() => {
            console.log('2-1');
            return 2;
        });

        const ajax3 = () => timeout(2000).then(() => {
            console.log('3-1');
            return 3;
        });

        const mergePromise = ajaxArray => {
            // 在这里实现你的代码
            var data = [];
            var sequence = Promise.resolve();
            ajaxArray.forEach(function(item){
                console.log("item",item);
                sequence = sequence.then(item).then(function(res){
                    console.log(res);
                    data.push(res);
                    return data;
                }); 
            })
            console.log("sequence",sequence)
            return sequence;
        };
        console.time("时间");
        mergePromise([ajax1, ajax2, ajax3]).then(data => {
            console.log('done');
        console.timeEnd("时间");
            console.log(data); // data 为 [1, 2, 3]
        });


#### 参考链接：
https://blog.csdn.net/u011500781/article/details/73883903