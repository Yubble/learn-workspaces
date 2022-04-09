<!-- 用以学习monorepo的workspace实现方法 -->

心得理解：

yarn workspace是lerna的语法糖，简化了lerna各式各样的操作语法。
它的原理则是将记录在package.json中workspace的每个package包都用软连接的方式安装到node_modules中，使得整体项目可以通过调用node_modules的方式随时调用子package包的内容，并且子仓库包中如果有内容修改，其他调用子仓库包的文件可以直接使用子仓库包中修改的内容，不需要编译！
依次极大地简化了调试过程。

这确实是现在最好用的monorepo方案。

<!-- workspace 的用法概览 -->

1、首先将目录的结构创建好，主要是编写各个子应用的文件夹，每个子目录要配置自己的package.json(跟目录下执行 'npm -init -w packages/子应用目录名 -y' 说明这个子应用是要注册进根目录的workspace中的)
2、再配置好根目录package.json的workspace字段，告诉根目录workspace在哪个目录下
3、下面各个子应用就可以编写自己的功能代码了
4、根目录下运行 npm i（此时发现子应用的代码都打包到node_modules中了）
5、这个时候子应用则可以相互调用，或者根目录下可以调用各个子应用（使用node_modules的调用方式）

<!-- 其他操作方法 -->
单独给某个模块安装依赖：
npm i 依赖名称 -w 需要安装的子应用

给所有子应用安装依赖：
npm i 依赖名称 -ws

移除某个依赖：
npm uninstall 依赖名称 -w 子应用

执行某个子应用中的脚本：
npm run dev -w 子应用