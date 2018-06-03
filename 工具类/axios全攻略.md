## axios全攻略

### 一、参考资料
>https://ykloveyxk.github.io/2017/02/25/axios%E5%85%A8%E6%94%BB%E7%95%A5/

### 二、如何安装
npm install axios

### 三、常见用法

1. get请求

        // 向具有指定ID的用户发出请求
        axios.get('/user?ID=12345')
        .then(function (response) {
            console.log(response);
        })
        .catch(function (error) {
            console.log(error);
        });
        


        // 也可以通过 params 对象传递参数
        axios.get('/user', {
            params: {
            ID: 12345
            }
        })
        .then(function (response) {
            console.log(response);
        })
        .catch(function (error) {
            console.log(error);
        });


2.post请求

    axios.post('/user', {
        firstName: 'Fred',
        lastName: 'Flintstone'
    })
    .then(function (response) {
        console.log(response);
    })
    .catch(function (error) {
        console.log(error);
    });

    