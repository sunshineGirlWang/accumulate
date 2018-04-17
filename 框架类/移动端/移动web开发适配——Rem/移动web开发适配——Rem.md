### 一、移动web开发与适配
1、特点

    A.跑在手机端的web页面（H5页面）
    B.跨平台
    C.基于webview
    D.告别IE拥抱webkit
    E.更高的适配和性能要求

2、常见移动web适配方法：

    (1)PC：
        A.960px/1000px  居中
        B.盒子模型，定高，定宽
        C.display：inline-block

    (2)移动web：
        A.定高，宽度百分比
        B.flex
        C.Media Query（媒体查询）

3、Media Query

    A.基本语法
        @media 媒体类型 and (媒体特性){
            /*css样式*/
        }
        媒体类型：screen,print...
        媒体特性：max-width,max-height...
    
    B.也可以使用link标签
         <link rel="stylesheet" type="text/css" href="" media="screen and (max-width: 320px)">

### 二、rem原理与简介
1、rem是 font size of the root element ,字体单位

2、特点：

    A.字体单位：
        值根据html根元素大小而定，同样可以作为宽度、高度等单位。
    B.适配原理：
        将px替换成rem，动态修改html的font-size适配。
    C.兼容性：
        IOS 6以上和andriod 2.1以上，基本覆盖所有流行的手机系统。

3、 使用js获取视窗宽度与高度

        A. //获取视窗宽度
        let htmlWidth = document.documentElement.clientWidth || document.body.clientWidth;
        
        B. //获取视窗高度
        let htmlDom = document.getElementsByTagName('html');

4、相关代码在

### 三、rem适配页面实战
1、rem与scss结合使用

    npm install node-sass -g   //安装
    node-sass a.scss b.css    //编译

2、采用rem高仿网易新闻H5版新闻列表页(相关代码在)

    A.页面框架搭建（构建，scss）
    B.页面样式编写
    C.rem计算代码编写
    D.适配多种机型大小，resize完善

