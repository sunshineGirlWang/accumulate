## NVM的常见命令
### 一、使用场景
    可以切换node的版本。

### 二、如何安装
>https://segmentfault.com/a/1190000007612011

### 三、常用命令
    (1)nvm install ## 安装指定版本，可模糊安装，如：安装v4.4.0，既可nvm install v4.4.0，又可nvm install 4.4

    (2)nvm uninstall ## 删除已安装的指定版本，语法与install类似

    (3)nvm use ## 切换使用指定的版本node

    (4)nvm ls ## 列出所有安装的版本

    (5)nvm ls-remote ## 列出所以远程服务器的版本（官方node version list）

    (6)nvm current ## 显示当前的版本

    (7)nvm alias ## 给不同的版本号添加别名

    (8)nvm unalias ## 删除已定义的别名

    (9)nvm reinstall-packages ## 在当前版本node环境下，重新全局安装指定版本号的npm包