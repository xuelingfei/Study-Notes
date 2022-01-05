## 安装VUE


- [直接引入](#直接引入)  
  - [CDN](#CDN)  
- [NPM 安装](#NPM-安装)  
- [命令行工具 (CLI)](#命令行工具-(CLI))  


### 直接引入
直接下载并用 `<script>` 标签引入，Vue 会被注册为一个全局变量。  
（开发版本包含完整的警告和调试模式，而生产版本删除了警告。）

#### CDN
对于制作原型或学习，可以使用最新版本：
```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

对于生产环境，推荐链接到一个明确的版本号和构建文件，以避免新版本造成的不可预期的破坏：
```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
```

对于原生 ES Modules，可以使用兼容 ES Module 的构建文件：
```html
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.esm.browser.js'
</script>
```


### NPM 安装
在用 Vue 构建大型应用时推荐使用 NPM 安装。  
首先 Vue 的安装依赖于 [node.js](https://nodejs.org/zh-cn/) ，查看 node.js 是否安装或者查看 node.js 版本：
```shell script
$ node -v
```
或
```shell script
$ node --version
```

安装 Vue：
```shell script
# 最新稳定版
$ npm install vue
```

查看 Vue 版本：
```shell script
$ vue -V
```
或
```shell script
$ vue --version
```


### 命令行工具 (CLI)
CLI (@vue/cli) 是一个全局安装的 npm 包，提供了终端里的 vue 命令。

安装：
```shell script
$ npm install -g @vue/cli
```
或
```shell script
$ npm install --global @vue/cli
```

卸载旧版本 (vue-cli)：
```shell script
$ npm uninstall vue-cli -g
```

升级：
```shell script
$ npm update -g @vue/cli
```


关于旧版本

Vue CLI 的包名称由 vue-cli 改成了 @vue/cli。 如果你已经全局安装了旧版本的 vue-cli (1.x 或 2.x)，你需要先通过 npm uninstall vue-cli -g 或 yarn global remove vue-cli 卸载它。

可以使用下列任一命令安装这个新的包：

npm install -g @vue/cli
# OR
yarn global add @vue/cli

安装之后，你就可以在命令行中访问 vue 命令。你可以通过简单运行 vue，看看是否展示出了一份所有可用命令的帮助信息，来验证它是否安装成功。

你还可以用这个命令来检查其版本是否正确：

vue --version


升级

如需升级全局的 Vue CLI 包，请运行：

npm update -g @vue/cli

# 或者
yarn global upgrade --latest @vue/cli


项目依赖

上面列出来的命令是用于升级全局的 Vue CLI。如需升级项目中的 Vue CLI 相关模块（以 @vue/cli-plugin- 或 vue-cli-plugin- 开头），请在项目目录下运行 vue upgrade：

用法： upgrade [options] [plugin-name]

（试用）升级 Vue CLI 服务及插件

选项：
  -t, --to <version>    升级 <plugin-name> 到指定的版本
  -f, --from <version>  跳过本地版本检测，默认插件是从此处指定的版本升级上来
  -r, --registry <url>  使用指定的 registry 地址安装依赖
  --all                 升级所有的插件
  --next                检查插件新版本时，包括 alpha/beta/rc 版本在内
  -h, --help            输出帮助内容


    用脚手架搭项目
    vue init webpack-simple (项目名字)
    　或　
    vue init webpack (项目名字)

两者区别就是
vue init webpack-simple :可以理解为轻巧的，没有多余的配置和包，但能保证项目正常运行。
vue init webpack : 可以理解为完整的，包含比较多配置和包。
————————————————
版权声明：本文为CSDN博主「大橙子额」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_43043994/article/details/82734436


Command vue init requires a global addon to be installed.Please run npm install -g @vue/cli-init and try again.



运行以下命令来创建一个新项目：

vue create hello-world

拉取 2.x 模板 (旧版本)

Vue CLI >= 3 和旧版使用了相同的 vue 命令，所以 Vue CLI 2 (vue-cli) 被覆盖了。如果你仍然需要使用旧版本的 vue init 功能，你可以全局安装一个桥接工具：

npm install -g @vue/cli-init
# `vue init` 的运行效果将会跟 `vue-cli@2.x` 相同
vue init webpack my-project


1.nrm

nrm(npm registry manager )是npm的镜像源管理工具，有时候国外资源太慢，使用这个就可以快速地在 npm 源间切换

2.安装nrm

在命令行执行命令，npm install -g nrm，全局安装nrm。

3.使用

执行命令nrm ls查看可选的源。
    nrm ls

    *npm ---- https://registry.npmjs.org/

    cnpm --- http://r.cnpmjs.org/

    taobao - http://registry.npm.taobao.org/

    eu ----- http://registry.npmjs.eu/

    au ----- http://registry.npmjs.org.au/

    sl ----- http://npm.strongloop.com/

    nj ----- https://registry.nodejitsu.com/

其中，带*的是当前使用的源，上面的输出表明当前源是官方源。


5.切换

如果要切换到taobao源，执行命令nrm use taobao。

6.增加

你可以增加定制的源，特别适用于添加企业内部的私有源，执行命令 nrm add <registry> <url>，其中reigstry为源名，url为源的路径。

    nrm add registry http://registry.npm.frp.trmap.cn/

7.删除

执行命令nrm del <registry>删除对应的源。

8.测试速度

你还可以通过 nrm test 测试相应源的响应时间。

    nrm test npm 

作者：西北码农
链接：https://www.jianshu.com/p/94d084ce6834
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

npm install 在安装 npm 包时，有两种命令参数可以把它们的信息写入 package.json 文件
一个是

 --save || -S // 运行依赖（发布）

    1

另一个是

–save-dev || -D //开发依赖（辅助）
区别是它们会把依赖包添加到package.json 文件

–save ： dependencies 键下，发布后还需要依赖的模块，譬如像jQuery库或者Angular框架类似的，我们在开发完后后肯定还要依赖它们，否则就运行不了。

–save-dev ： devDependencies 键下，开发时的依赖比如安装 js的压缩包gulp-uglify 因为我们在发布后用不到它，而只是在我们开发才用到它。


yarn add <package...>

This will install one or more packages in your dependencies.
yarn add <package...> [--dev/-D]

Using --dev or -D will install one or more packages in your devDependencies.
