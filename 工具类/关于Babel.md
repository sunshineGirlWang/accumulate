参考资料：
>https://zhuanlan.zhihu.com/p/43249121

#### 一、使用方法
总共存在三种方式：

1、使用单体文件 (standalone script)

2、命令行 (cli)

3、构建工具的插件 (webpack 的 babel-loader, rollup 的 rollup-plugin-babel)。

其中后面两种比较常见。第二种多见于 package.json 中的 scripts 段落中的某条命令；第三种就直接集成到构建工具中。

这三种方式只有入口不同而已，调用的 babel 内核，处理方式都是一样的，所以我们先不纠结入口的问题。

