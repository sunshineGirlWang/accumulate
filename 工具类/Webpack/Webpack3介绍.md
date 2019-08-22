#### 1、webpack，gulp/grunt，npm，它们有什么区别?
	A.webpack 是模块打包器（module bundler），把所有的模块打包成一个或少量文件，
	使你只需加载少量文件即可运行整个应用，而无需像之前那样加载大量的图片，css文件，js文件，字体文件等等。

    B.gulp／grunt 是自动化构建工具，或者叫任务运行器（task runner），
    是把你所有重复的手动操作让代码来做，例如压缩JS代码、CSS代码，代码检查、代码编译等等，
    自动化构建工具并不能把所有模块打包到一起，也不能构建不同模块之间的依赖图。

    两者来比较的话，gulp/grunt 无法做模块打包的事，webpack 虽然有 loader 和 plugin可以做一部分 gulp／grunt 能做的事，
    但是终究 webpack 的插件还是不如 gulp／grunt 的插件丰富，能做的事比较有限。
    于是有人两者结合着用，将 webpack 放到 gulp／grunt 中用。

    然而，更好的方法是用 npm scripts 取代 gulp／grunt，npm 是 node 的包管理器 (node package manager)，
    用于管理 node 的第三方软件包，npm 对于任务命令的良好支持让你最终省却了编写任务代码的必要，
    取而代之的，是老祖宗的几个命令行，仅靠几句命令行就足以完成你的模块打包和自动化构建的所有需求。
   
#### 2、基本配置
	A.npm init -y   //生成了一个package.json，里面
    B.npm i --save-dev webpack  //生成了一个node_modules，里面包含了webpack
    C.npm i --save lodash   //工具函数库 lodash 来演示
    
    echo.>webpack.config.js