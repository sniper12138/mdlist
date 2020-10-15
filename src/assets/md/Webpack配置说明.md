# Webpack配置说明

这里展示的一个Webpack项目最基础的文件与文件夹结构，以及Webpack配置说明

## **项目目录结构**

```
configs //存放与webpack配置相关的配置
   |--webpack.base.js //webpack公用的基础配置
   |--webpack.dist.js //webpack测试环境打包的配置
   |--webpack.params.js //项目单独配置，比如CDN和HOST那些
   |--webpack.release.js //webpack正式环境打包的配置
   |--webpack.start.js  //webpack开发调试环境的配置
   package.json //webpack的依赖配置（默认不需要动）
   webpack.config.js //webpack启动的配置入口文件
   src    //存放该项目源码
   |--css  //存在所有less文件和css文件
   |--img  //存放所有图片文件
   |--inline  //存在所有需要被引用的模块html文件（例如多个页面公用的header，footer等等，或者是一个页面太长了，将各自模块放于此）
   |--global_include //需要被其他项目include的文件
   |--template //模板文件，webpack改用art-template，页面不需要额外引用模板js
   |--js    //所有js文件
   |    |--entry //页面的入口文件，名字与页面一一对应
   |    |--app   //与项目逻辑相关的js文件，例如`index.js`
   |    |--common   //项目中公用的js文件
   |    |--lib     //项目中需要用到的第三方库与插件，例如`baiduTemplate.js`，`jquery.slider.js`等
   |--index.html     //项目中需要的页面html文件，名字与js/entry下的对应
```

## **项目发布后的结构**

```
dist    //存放需要发布的文件
|--css    //这里是所有编译好的less转css的文件
|--img     //所有需要发布的img
|--js    //这里是已经自动打包合并好的js和css文件，以及lib合并文件
|--index.html     //修改完毕的html
```

## **Webpack相关知识**

如何安装Webpack，如何使用来开发，如何使用来打包发布等

### <br>1.安装Webpack与相关插件

-   安装Nodejs，直接到官网下载安装<http://nodejs.org/>，目前只能安装v6.11以上版本，而且只能是v6系列

-   安装webpack，在命令行下，输入：

```
npm install -g webpack webpack-cli webpack-dev-server //webpack核心、命令行工具、调试服务
```

-   安装webpack需要的插件(下面路径中用户名为各自电脑的用户名)

-   拷贝webpackDemo中的package.json文件到
    C:\\Users\\用户名\\AppData\\Roaming\\npm下

-   在命令行中输入：npm install进行插件安装

-   添加名为：NODE_PATH，值为：C:\\Users\\用户名\\AppData\\Roaming\\npm\\node_modules到系统的环境变量中

-   进入C:\\Users\\用户名\\AppData\\Roaming\\npm\\node_modules\\webpack\\node_modules\\enhanced-resolve\\lib\\路径

-   修改ModulesInHierachicDirectoriesPlugin.js文件，在第26行后面回车，就是forEachBail循环前面，添加代码addrs.push(process.env.NODE_PATH);

-   重新打开cmd命令行，即可全局使用webpack

-   Mac安装方法请看此云笔记[mac下修改全局webpack支持操作方法](http://res.nie.netease.com/comm/doc/tools/Mac%E8%AE%BE%E7%BD%AE%E5%85%A8%E5%B1%80Webpack.html)

### 2.如何使用Webpack

-   html文件，需要放在src根目录下

```
//表示将此html文件内容引用到该标签位置
<link rel="import" href="inline/index.html?__inline"&
//项目的css与js文件，不需要单独引用，webpack会打包
```

-   css和less文件

    -   需要自动合并为雪碧图的图片，在引用文件名后添加?__sprite，而且路径必须带双引号，单个css文件中所有添加此标记的，会合并成一个单独文件

    -   那些被合并的原图，文件名必须以下划线_开头，这样打包发布，就不会出现未合并之前的图片，详情参考源码

-   js文件

    -   entry下的入口js文件，需要单独引用页面的less和css文件

    -   每一个单独的js文件最终都会打包成为一个单独模块，变量也是模块内部变量

    -   项目内模块加载使用import方式

    -   组件加载废弃nie.require方式，采用nie.pack

-   可以使用ES6、7语法特性

```
//引用CSS文件
require(‘css/index.less’);
//引入html文件，最终返回html内容的字符串
import inline_html from ‘../../inline/header.html’;
//js中，需要引用到资源的文件，需要这样写，打包时候才会替换为CDN路径
import img_url from ‘../../img/demo.png’;
//引用art-template模板文件，模板语法为js原生语法
let under = require(“../../template/header.art”);
let html = under({list : [1,2,3]});
//引用项目内其他模块
import Common from ‘../app/index.js’;
//加载组内的组件，兼容ie8,9浏览器
nie.pack([“nie.util.videoV2”,”nie.util.fur3”]).then((rets)=&{
//获取组件对应的引用，名字与组件最后一个单词相同
let {videoV2,fur3} = rets;
});
 
 
//加载组内的组件方式2，只支持ie10，chrome，移动端
async init(){
//获取组件引用
let {fur3,videoV2} = await nie.pack(["nie.util.fur3","nie.util.videoV2"]);
}
init();
 
 
//默认自带两个变量
let isDebug = DEBUG ; //编译打包后，测试环境会变为true，正式环境为false
let cdn = CDNPATH ; //编译后，会变成对应的cdn-path值
//返回模块接口
export default {
init : init,
show : show
}
```

    **如何开始Webpack**

    进入项目的文件夹，输入命令：
    
```
npm run start //启动webpack服务，并且监听文件变化，自动编译，刷新浏览器，服务默认：http://127.0.0.1:9000
npm run dist //打为测试环境的包
npm run release //打为发布环境的包
```

## **注意事项**

-   使用webpack，不支持IE6、7浏览器

-   尽量使用ES6的特性

-   强制模块化开发，一个文件即一个模块

-   默认开启热更新模式，修改html文件不会刷新页面和内容，修改js与less则只刷新对应的内容，不刷页面；需要刷新，请在webpack.start.js中去掉hot:true

-   node_modules禁止提交上git

-   需要针对项目的特殊，修改配置文件

-   大部分的FIS插件还未实
