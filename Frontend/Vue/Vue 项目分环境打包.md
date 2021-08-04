## Vue 项目分环境打包


- [第1步：安装 cross-env](#第1步安装-cross-env)  
- [第2步：修改各环境下的参数](#第2步修改各环境下的参数)  
- [第3步：修改项目 package.json 文件](#第3步修改项目-packagejson-文件)  
- [第4步：修改 config/index.js](#第4步修改-configindexjs)  
- [第5步：在 webpackage.prod.conf.js 中使用构建环境参数](#第5步在-webpackageprodconfjs-中使用构建环境参数)  
- [第6步：调整 build/build.js](#第6步调整-buildbuildjs)  


#### 第1步：安装 cross-env
```shell script
npm i --save-dev cross-env
```


#### 第2步：修改各环境下的参数
在 `config/` 目录下添加 test.env.js 和 prod.env.js  
修改 prod.env.js 里的内容，修改后的内容如下：
```js
'use strict'
module.exports = {
  NODE_ENV: '"production"',
  ENV_CONFIG:'"prod"',
  BASE_API: '"http://bseosback.bseos.com"', //以bseos为例，替换为自己项目地址
  LOGIN_API:'"http://www.bseos.com"'
}
```
对 test.env.js 文件内容进行修改，修改后的内容如下：
```js
'use strict'
module.exports = {
  NODE_ENV: '"testing"',
  ENV_CONFIG:'"test"',
  BASE_API: '"http://10.197.190.62:81"',
  LOGIN_API:'"http://10.197.190.62"'
}
```
对 dev.env.js 文件内容进行修改，修改后的内容如下。dev 环境配制了服务代理，API_ROOT 前的 api 是配制的代理地址。
```js
'use strict'
var merge = require('webpack-merge')
var prodEnv = require('./prod.env')

module.exports = merge(prodEnv, {
  NODE_ENV: '"development"',
  ENV_CONFIG:'"dev"',
  BASE_API: '"http://10.197.190.62:81"',
  LOGIN_API:'"http://10.197.190.62"'
})
```


#### 第3步：修改项目 package.json 文件
对 package.json 文件中的 `scripts` 内容进行个性，添加上新定义的几种环境的打包过程，里的参数与前面的调协保持一致。
```json
"scripts": {
  "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
  "start": "npm run dev",
  "build": "node build/build.js",
  "build:test": "cross-env NODE_ENV=production env_config=test node build/build.js",
  "build:prod": "cross-env NODE_ENV=production env_config=prod node build/build.js"
},
```


#### 第4步：修改 config/index.js
修改 config/index.js 文件中 `build` 参数，这里的参数会在 build/webpackage.prod.conf.js 中使用到
```js
build: {
  // 添加test prod 三处环境的配制
  prodEnv: require('./prod.env'),
  testEnv: require('./test.env'),

  index: path.resolve(__dirname, '../dist-'+process.env.env_config+'/index.html'),
  assetsRoot: path.resolve(__dirname, '../dist-'+process.env.env_config),

  //其它内容不需要做任何调整的内容 ...
}
```


#### 第5步：在 webpackage.prod.conf.js 中使用构建环境参数
对 build/webpackage.prod.conf.js 文件进行修改，调整 `env` 常量的生成方式。个性 `env` 常量的定义：
```js
// const env = require('../config/prod.env')
const env = config.build[process.env.env_config+'Env']
```


#### 第6步：调整 build/build.js
删除 `process.env.NODE_ENV` 的赋值，修改 `spinner` 的定义，调整后的内容如下：
```js
'use strict'
require('./check-versions')()
// 注释掉的代码
// process.env.NODE_ENV = 'production'
const ora = require('ora')
const rm = require('rimraf')
const path = require('path')
const chalk = require('chalk')
const webpack = require('webpack')
const config = require('../config')
const webpackConfig = require('./webpack.prod.conf')
// 修改spinner的定义
// const spinner = ora('building for production...')
var spinner = ora('building for ' + process.env.NODE_ENV + ' of ' + process.env.env_config+ ' mode...' )
//其它内容不需要做任何调整的内容 ...
```
