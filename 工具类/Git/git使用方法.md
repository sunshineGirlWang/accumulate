一、上传代码
第一步：建立git仓库
cd到你的本地项目根目录下，执行git命令，此命令会在当前目录下创建一个.git文件夹。
git init

第二步：将项目的所有文件添加到仓库中
git add .
这个命令会把当前路径下的所有文件，添加到待上传的文件列表中。
如果想添加某个特定的文件，只需把.换成特定的文件名即可

第三步：将add的文件commit到仓库
git commit -m "注释语句"

第四步：去github上创建自己的Repository，点击NewRepository。
点击Create repository，就会进入到类似下面的一个页面，拿到创建的仓库的https地址

第五步：将本地的仓库关联到github上
git remote add origin https://自己的仓库url地址

第六步，上传代码到github远程仓库
git push -u origin master

执行完后，如果没有异常，等待执行完就上传成功了，中间可能会让你输入Username和Password，你只要输入github的账号和密码就行了.

二、全局设置用户名及邮箱
git config user.name "Your Name" 
git config user.email you@example.com

三、下载代码
git  clone  XXXXXX(代码的url)

