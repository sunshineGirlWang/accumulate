### webpack中devtool的sourceMap

#### 一、作用
    sourceMap可以帮助我们在开发过程中调试代码。

#### 二、几种模式
1、eval

    每个 module 会封装到 eval 里包裹起来执行，并且会在末尾追加注释 //@ sourceURL.

2、source-map

    生成一个 SourceMap 文件.

3、hidden-source-map

    和 source-map 一样，但不会在 bundle 末尾追加注释.

4、inline-source-map

    生成一个 DataUrl 形式的 SourceMap 文件.

5、eval-source-map

    每个 module 会通过 eval() 来执行，并且生成一个 DataUrl 形式的 SourceMap .

6、cheap-source-map

    生成一个没有列信息（column-mappings）的 SourceMaps 文件，不包含 loader 的 sourcemap（譬如 babel 的 sourcemap）

7、cheap-module-source-map

    生成一个没有列信息（column-mappings）的 SourceMaps 文件，同时 loader 的 sourcemap 也被简化为只包含对应行的。

#### 三、注意点
1、webpack 不仅支持这 7 种，而且它们还是可以任意组合上面的 eval、inline、hidden 关键字，就如文档所说，你可以设置 souremap 选项为 cheap-module-inline-source-map。

2、如果你的 modules 里面已经包含了 SourceMaps ，你需要用 source-map-loader  来和合并生成一个新的 SourceMaps 。

#### 四、哪种模式比较好
1、总结:

    A.开发环境推荐：
        cheap-module-eval-source-map

    B.生产环境推荐：
        cheap-module-source-map （这也是下版本 webpack 使用-d命令启动 debug 模式时的默认选项）

2、原因：

    (1)使用 cheap 模式可以大幅提高 souremap 生成的效率。大部分情况我们调试并不关心列信息，而且就算 sourcemap 没有列，有些浏览器引擎（例如 v8） 也会给出列信息。

    (2)使用 eval 方式可大幅提高持续构建效率。参考官方文档提供的速度对比表格可以看到 eval 模式的编译速度很快。

    (3)使用 module 可支持 babel 这种预编译工具（在 webpack 里做为 loader 使用）。

    (4)使用 eval-source-map 模式可以减少网络请求。这种模式开启 DataUrl 本身包含完整 sourcemap 信息，并不需要像 sourceURL 那样，浏览器需要发送一个完整请求去获取 sourcemap 文件，这会略微提高点效率。而生产环境中则不宜用 eval，这样会让文件变得极大。

