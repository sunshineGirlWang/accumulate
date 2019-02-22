@Author:wang-qingqing O(∩_∩)O 

@AddTime:20190222

@参考资料：
    https://www.jianshu.com/p/e709b71f12da

### 常用命令
1、安装
    npm install pm2 -g  //全局安装pm2
    pm2 update    //更新pm2
    pm2 uninstall pm2 //移除pm2

2、开启关闭
    pm2 start server.js   //启动server.js进程
    pm2 start server.js  -i 4  //启动4个server.js进程
    pm2 restart server.js   //重启server.js进程
    pm2 stop all      //停止所有进程
    pm2 stop server.js  //停止server.js进程
    pm2 stop 0    //停止编号为0的进程

3、配置启动信息
    //创建app.json，内容如下
    {
        "apps":[{
            "script": "server.js", //进程名
            "instances": "max", //开启进程数，可为数组，也可为max。与服务器cpu核数相关。
            "exec_mode": "cluster" //可选：fork（服务器单核推荐）  cluster（多核推荐）    
        }]
    }

    pm2 start app.json

4、查看
    pm2 list  //查看当前正在运行的进程
    pm2 show 0  //查看执行编号为0的进程

5、实时监控
    pm2 monit //监控当前所有的进程
    pm2 monit 0 //监控执行编号为0的进程
    pm2 monit server.js  //监控名称为server.js的进程

6、日志
    pm2 logs //显示所有日志
    pm2 logs 0  //显示执行编号为0的日志
    pm2 logs server.js  //显示名称为server.js的进程
    pm2 flush  //清洗所有的数据