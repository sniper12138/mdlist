# PC端HTML/CSS规范

本文档概述PC端页面制作规范，主要为了更好的团队协作，保证前端实现的一致性，提升网站的可维护性。

## **总则**

-   统一使用UTF-8编码，除非是旧站点而且需要用到include顶条的用GBK(GB2312)。

-   命名使用英文小写，词组用'-'连接，力争做到贴切且见名知意。

-   代码应添加必要的注释，注释有意义且易懂。

-   代码需做到易于他人阅读的，便于维护管理。

-   代码版本管理统一使用git。

## **兼容问题**

页面大小:

-   【PC端】最小分辨率测试，放弃1280 *800 ，改为 1440* 900。

-   【移动端】单屏切换滑动的专题，放弃
    iphone5和se的兼容，最小兼容到iphone6标准。

浏览器:

-   【PC端】IE10-11、chrome、QQ、360、Safari、edge、搜狗（抽查）。

-   【移动端】iphone、华为、oppo、vivo、小米、三星 等手机中的
    自带和微信、UC和QQ（抽查）。

## **移动、PC跳转**（正常情况）

```
<script type="text/javascript">
  !function(){
    function params(u, p){
      var m = new RegExp("(?:&|/?)"+p+"=([^&$]+)").exec(u);
      return m ? m[1] : '';
    }
    if(/iphone|android|ipod/i.test(navigator.userAgent.toLowerCase()) == true && params(location.search, "from") != "mobile"){
      location.href = '移动端地址';
    }
  }();
</script><!--ignore-->
```

移动端需要判断是PC打开，自动跳转到PC页面，请将下面代码完整加在head标签css引入之前，不要漏了\<!--ignore--\>

```
<script type="text/javascript">
  !function(){
    if(/iphone|android|ipod/i.test(navigator.userAgent.toLowerCase()) == false && params(location.search, "from") != "desktop"){
      location.href = 'PC地址';
    }
  }();
</script><!--ignore-->
```

## **ipad OS 强制跳转移动端代码**（特殊情况）

PC端页面，请将下面代码完整加在head标签css引入之前，不要漏了\<!--ignore--\>

```
<script type="text/javascript">
      (function(){
          if (/(iPhone|iPod|iPad|Android)/i.test(navigator.userAgent)||(/Mac OS X/i.test(navigator.userAgent)&&typeof window.DeviceOrientationEvent!="undefined")) {
 
              window.location.href="移动端地址";
          }
      }());
  </script><!--ignore-->
```

移动端页面，请将下面代码完整加在head标签css引入之前，不要漏了\<!--ignore--\>

```
<script type="text/javascript">
        (function(){
 
            if (!/(iPhone|iPod|iPad|Android)/i.test(navigator.userAgent)&&!(/Mac OS X/i.test(navigator.userAgent)&&typeof window.DeviceOrientationEvent!="undefined")) {
 
               window.location.href="PC地址";
            }
        }());
    </script><!--ignore-->
```

## **文件命名**

-   所有文件名统一使用小写

-   文件名禁止以数字开头

-   HTML 首页、引导页命名为index.html

-   HTML 专题内页，有明显分类，按英文 或
    拼命首字母命名+首字母+尾字母，如：标题为“这是一个充值活动”，页面名为“zsygczhdzd.html”。

-   HTML
    专题内页，有明显分类、带参数，如：标题为“充值活动-充值返利”，页面名为“czhdcd.html?page=czflcl”或“czhdcd.html\#page=czflcl”。

-   HTML
    专题内页，无明显分类、带参数，如：只有一个内页通过参数展示不同内容，页面名为“page.html?page=czflcl”或“page.html\#page=czflcl”

# **HTML规范**

## **编写原则**

-   遵循w3c标准，构建良好的 HTML 结构和语义。

-   保证 HTML 代码整洁工整，结构清晰。可使用相关工具格式化

-   HTML 里的类名、ID 名，全部小写，词组用’-‘连接

-   静态页面必须做好足够详细的注释。

-   推荐将所有JS放到页面底部。

### HTML代码模板示例

```
<!DOCTYPE html>
 
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<!--include virtual="/global_include/config.html"-->
<title><!--#echo var='Title'--></title>
<meta name="keywords" content="<!--#echo var='Keywords'-->" />
<meta name="description" content="<!--#echo var='Description'-->" />
<link rel="dns-prefetch" href="">
 
<!-- 适配跳转代码 -->
<script type="text/javascript" data-fixed="true">
  !function(){
    function params(u, p){
      var m = new RegExp('(?:&|/?)'+ p +'=([^&$]+)').exec(u);
      return m ? m[1] : '';
    }
    if(/iphone|ios|android|ipod/i.test(navigator.userAgent.toLowerCase()) == true && params(location.search, "from") != "mobile"){
      location.href = '移动端地址';
    }
  }();
</script><!--ignore-->
 
<!-- 样式 -->
<!--usemin css(pkg/boot.css)-->
<link rel="stylesheet" type="text/css" href="css/index.less" />
<!--end-->
 
<!--[if lt IE 9]>
<script src="/comm/html5/html5shiv.js"></script>
<![endif]-->
</head>
<body>
  <!--分享信息-->
  <div id="share_content" style="display:none">
    <div id="share_title" pub-name="分享文案" ></div>
    <div id="share_url" pub-name="分享地址" ></div>
    <div id="share_desc" pub-name="分享文本" ></div>
    <img id="share_pic" data-src="#" pub-name="分享图片" />
  </div>
 
  <!--SEO start-->
    <h1 class="hide"></h1>
  <!--SEO end-->
 
  <!--【顶条】官网首页统一使用include，其他页面采用<div id="NIE-topBar"></div>的方式-->
  <!--#include virtual="/global_site_inc/topBar-include.html"-->
 
  <div class="wrap">
 
  </div>
 
  <!--版权-->
  <div id="NIE-copyRight"></div>
 
<!-- jquery mix NIE (最新版本）-->
<script src="/comm/js/jquery(mixNIE).1.11.js"></script>
 
<!--usemin js(pkg/index.js)-->
<script src="js/app/index.js"></script>
<!--end-->
</body>
</html>
```

**doctype文档类型**

```
<!DOCTYPE html>
<!--注：文档类型统一使用html5的doctype-->
```

**页面编码**

```
<meta charset="utf-8">
<!--注：编码统一使用utf-8-->
```

## **页面title规则**

页面类型

title

栏目页

游戏名+栏目名_网站名称

文章页

文章标题名_网站名称

专题页首页

游戏名+专题名+营销词_网站名称

专题页内页

游戏名+内页主题*专题名*网站名称

## **页面title,keywords,description**

SEO重要信息，需编辑提供，如编辑无提供，需自行编写

```

<title></title>
 
<meta name="description" content="" />
 
<meta name="keywords" content="" />
 
<!--
注：如编辑无提供，可按照如下方式自行编写:
title为页面标题
Keywords为产品名、专题名、专题相关名词，之间用英文半角逗号隔开。
description值一般为页面标题或主题，针对该页面主题的说明。
-->
```

## **global_include规范说明**

为了统一include的管理，所以，只有官网(PC和移动单独分开)才能有global_include文件夹，其余一律用inline方式实现

-   如果是新产品没有config.html的话，请新建一个config.html页面，在页面创建三个变量供页面调用，包括title、keywords、description，参考如下：

-   页面创建好了之后在FTP根目录下新建一个文件夹global_include，将config.html上传到这个文件夹

-   页面引入：在titile之前include页面，title及meta部分使用变量来引入SEO的内容,可适当加前缀，如：

```
<title>内页标题_</title>
<meta name="keywords" content="" />

<meta name="description" content="" />
```

### Author（fis项目会自动添加）

```
<meta name="author" content="小虎，TinyTiger.cn" />
```

### Copyright（fis项目会自动添加）

```
<meta name="copyright" content="小虎，TinyTiger.cn" />
```

### 页面说明注释（fis项目会自动添加）

为方便在产品环境中的查看，在head区域中加上对页面信息和相关人员注释：CP，不能为其他内容。相关信息可上PM提取

```
<meta name="pmid" content="" />
 
<meta name="editor" content="" />
 
<meta name="designer" content="" />
 
<meta name="front-end technicist" content="" />
```

### 预先获取资源服务器地址

为减少DNS的请求次数，需进行资源服务器DNS预先获取，如：

```
<link rel="dns-prefetch" href="https://play.tinytiger.cn">
```

### CSS引入方式

统一使用外链CSS文件方法，例如：

```
<link type="text/css" rel="stylesheet" href="https://play.tinytiger.cn/gw/13v1/css/index.v1015.css?v=1" media="all" />
```

  
注：

1.  在head区域内引用。

2.  所有页面尽量都基于同一套css，部分特殊页面样式可以有单独css文件，css文件之间的规则没有覆盖；

3.  不推荐在css里import其他css。如：

```
\@import url('b.css');
```

### css reset示例

新项目统一使用fis自动化工具构建，css的reset推荐使用fis项目的中[\_reset.less](http://git-wz.gz.netease.com/nie-comm/fis-project-demo/blob/master/src/css/_reset.less)

也可使用线上的css reset

```
<link type="text/css" rel="stylesheet" href="https://play.tinytiger.cn/comm/css/base.css" media="all" />
```

### JS框架的引用

统一使用以下公共JS

```
<script src="https://play.tinytiger.cn/comm/js/jquery(mixNIE).1.11.js"></script>
```

### 基本字体

```
body{font-family:"Microsoft YaHei",simSun,"Lucida Grande","Lucida Sans Unicode",Arial;font-size:12px;}
```

### 顶条

-   官网首页顶条统一使用include，禁止使用旧的方式。

-   但其他栏目页、文章页、专题等等，这些页面还是使用旧方式：
    由于顶条样式是通过js调用的，打开页面后需要时间加载，在加载完成后顶条高度会发生变化，导致页面会出现跳动情况，可在在本页面调用css里定义顶条高度为55px。

```
<!-- topbar start -->
<div id="NIE-topBar"></div>
<!-- topbar end   -->
```

-   如果是fis项目，需要在fis-conf.js配置文件里配置include文件域名需要指向。 如：
    //修改include文件的域名
    fis.config.set(‘include-host’,’https://play.tinytiger.cn‘) )

-   非fis项目，本地调试的时候请自行建立/global_site_inc/topBar-include.html文件，并启用include，切记上传时不要上传该文件

### 版权

```
<!--版权-->
<div id="NIE-copyRight"></div>
```

-   版权可根据设计调整

-   默认颜色，适合浅色背景，无需特别设置

-   灰色，适合较深色背景 设置方法：在js添加以下代码
    nie.config.copyRight.setGray();

-   白色，适合深色背景 设置方法：在js添加以下代码
    nie.config.copyRight.setWhite();

### 链接

-   必须给链接加地址

-   同一个专题，或同风格页面之间的跳转，当前页打开

-   不同产品，不同专题，不同页面风格之间跳转，可新窗口打开

-   链接如果是触发js函数的，禁止使用\#号，而需要用javascript:void(0)

-   链接形式的按钮，都必须至少拥有hover或者active态

-   产品要求点击提示”敬请期待！”的，代码统一用href=”javascript:alert(‘敬请期待！’)”的形式

-   在“进入官网”的链接中，必须加上页面文件地址，如：”[https://xxx.tinytiger.cn/index.html”，防止有引导页情况下，无法通过链接访问官网首页。](https://xxx.163.com/index.html%22%EF%BC%8C%E9%98%B2%E6%AD%A2%E6%9C%89%E5%BC%95%E5%AF%BC%E9%A1%B5%E6%83%85%E5%86%B5%E4%B8%8B%EF%BC%8C%E6%97%A0%E6%B3%95%E9%80%9A%E8%BF%87%E9%93%BE%E6%8E%A5%E8%AE%BF%E9%97%AE%E5%AE%98%E7%BD%91%E9%A6%96%E9%A1%B5%E3%80%82)

### A标签规范

-   不管 HTML书写 或者 JS插入，A标签必须
    PC端填写内容、移动端添加title属性，无论是否带调转链接。纯背景图片A标签，内容必须准确表达图片内容，如：“BANNER切换按钮”、“梦幻西游LOGO”、“返回顶部”，可通过CSS控制
    “text-indent:-9999px;overflow:hidden”。

-   由于 text-indent:-9999px;
    在UC下会影响整个页面的DOM结构定位，移动端添加title属性

-   IMG标签，都需要添加alt属性，表明图片的作用，例如：梦幻logo、梦幻N周年庆典

### HTML 注释

-   需对模块进行注释，标明模块的作用。

-   关键及需要注意的地方需要添加注释。

**HTML 注释范例1**

```
<!--引人左侧模块-->
<link rel="import" href="inline/leftside.html?__inline">
```

**HTML 注释范例2**

```
<div class="list" id="top5">
  <!--票数最多的5个-->
</div>
```

**HTML 注释范例3**

```
<!--导航 Start-->
<ul class="nav">
  xxxx
</ul>
<!--导航 end-->
```

### 常用标签

标签的使用应遵循w3c标准，按实际语义需要选用最符合要求的标签。

标签名称

标签语义

备注

form

表单

为方便对齐，建议表单使用表格嵌套

table

表格

主要用于展现数据，以及套表单。 禁止使用表格布局

input

输入控件

类型有text,button,checkbox,radio,file,hidden,image,password,reset,submit

button

按钮

p

段落

常用于文章段落

a

超链接

为每个链接加上title属性

div

无语义

块标签

span

无语义

内联标签

h1—-h6

标题

SEO权重高，一个页面只能有一个H1标签

img

图片

为每个图片加上alt属性，大图片加上宽和高

ol

有序列表

ul

无序列表

dl

定义列表

label

为其它元素指定标签

strong

强调

b

加粗

\> 常用html5标签： 使用HTML5更容易表达语义

标签名称

标签语义

备注

标签名称

标签语义

备注

header

头部

nav

导航

footer

底部

section

内容区块

article

文章内容

aside

相关信息

video

定义视频

audio

定义音频

canvas

定义画布

### 注意事项

-   正确嵌套标签，尽量减少标签嵌套，禁止使用内联标签包裹块级标签。

-   所有标签都要闭合，以开始标签起始，以结束标签终止。

-   标题根据重要性用hx(同一页面只能有一个h1标签)。

-   标签名和属性名必须用小写，属性值应该被包括在引号内。

-   HTML 里的类名、ID 名，全部小写，词组用’-‘连接。

-   不涉及JS的元素不占用ID属性，ID属性留给开发人员或者脚本编写人员使用，应用样式用class。

-   如需使用ID，一个ID在一个页面必须是唯一，不能重复。

-   HTML 页面结构统一，便于提取出统一的模版。

-   套页面中相同的设计部分（比如版头、在线帮助）用相同的html代码和嵌套结构。

-   同需求的页面尽量都基于同一套css，部分特殊页面样式可以有单独css文件，css文件之间的规则没有覆盖。

-   分享文案请放在页面上，方便修改。

-   移动端分享图片如果较大 请预先缓存。

其他： \> 代码内不允许有无意义空格，换行，tab缩进。

## **Css规范**

### Css文件目录规范

-   Css文件放置于一个Css文件下，根据项目的复杂度，进行文件夹和文件划分

-   Css文件命名使用英文小写，词组使用'-'链接，做到见名知意，简洁易懂
    例如：indexp-page.css

-   所有的Css上线时需放到资源服务器

### Css注释规范

#### 顶部注释

顶部注释是为了提供文件作用以及文件版本信息等，让使用者能快速了解文件作用

例如：

```
/*
 * @description:中文说明
 * @author:xxxxx
 * @update : xxxx (2014.10.20)
 */
```

#### 行间注释

行间注释文字不宜过多

例如：

```
/*我是注释…*/
```

块注释或多行注释使用

例如：

```
/*
* 注释第一行
* 注释第二行
*/
```

#### 模块注释

模块注释使用在单个模块中，作用是解释模块功能或者模块使用场景、使用方式等等

例如：

```
/** 通用header样式模块 **/
 
xxxxxx
 
xxxxxxx
 
xxxxxxx
 
/**通用header样式模块 结束**/
```

#### Bugfix

用于注释修改记录，有利于协作开发时提高效率 例如：

```
/* TODO: xxxx by name 2014-10-20 */
 
/* BUGFIX: xxxx by name 2014-10-20 */
```

### Css命名规范

-   使用类class选择器，不推荐使用id选择器

-   类名尽量简洁易懂，忌讳使用晦涩抽象的命名，class只要足够表达含义，越短越好。这样既能提高代码效率。例如.navigator{}.atr{}（不推荐）.nav{}.author{}（推荐）

-   规则命名中，一律采用英文小写，词组用'-'连接，做到见名知意，简洁易懂，一般来说，不建议使用太长的class名称，比如.gamedetail-subname，一般来说，不要超过三层

-   类选择器避免同时使用标签span.calss，从性能上考虑，也要尽量减少选择器的层级太深，一般最多三层

-   分类的命名方法使用带个字母为前缀，比如.g。

### Css常用语义命名

#### 布局类：（.g）

布局类

布局类

布局类

布局类

容器：containe

页头：header

内容：content

主体：main

页尾：footer

侧栏：sidebar

控制页面宽度容器：wrapper

左右中：left right center

#### 模块类:（.m）

模块类

模块类

模块类

模块类

模块类

模块类

导航：nav

子导航：subnav

菜单：menu

选项卡：tab

标题：title

列表：list

表格：form

列表：list

热点：hot

登陆：login

标志：logo

广告：ad

搜索：sch

幻灯片：sld

提示：tips

帮助：hlp

新闻：news

下载：dld

投票：vote

版权：cprt

结果：rst

标题：title/tt

按钮：btn

输入：ipt

#### 功能类：（.f）

功能类

功能类

功能类

功能类

功能类

清除浮动：cb

向左浮动：fl

向右浮动：fr

内联块级：ib

文本居中：tc

文本右对齐：tr

文本左对齐：tl

溢出隐藏：oh

字体大小：fs

字体粗细：fw

#### 状态类：（.z）

状态类

状态类

状态类

状态类

选中：sel

当前：cur

显示：show

隐藏：hide

打开：open

关闭：close

出错：err

不可用：disa

### Css性能优化

-   尽量简化css书写，例如：magin-top:10px;margin-right:10px;直接写成margin:10 10
    0 0;

-   class命名在满足功能的基础上尽量简短，减少无用选择器嵌套，避免查找消耗。但要注意避免覆盖其他样式。如能够直接使用.nav
    就不要使用.header .nav

-   不要使用低性能的选择器。如全局通配符 \*

-   除非必须，class和id前不写对应的标签

-   0不需要单位，可以省略，比如0px可以写成0，0.8pt可以写成.8pt

-   用16进制表示颜色时，尽量简化，比如\#FFAABB可以简化\#FAB

-   没有边框，不要写border：0，应该写border：none

-   尽量避免使用AlphaImageLoader

-   尽量避免使用绝对定位和相对定位

-   在保持代码解耦的前提下，尽量合并重复样式

### Css Hack

\> 原则上不允许使用css
hack，很多兼容问题可以通过改变方法和思路来解决，不一定需要hack，完全可以根据经验绕过兼容问题，一个合理的结构和布局是极少会碰到兼容问题，由于浏览器自身缺陷我们无法避开，可以允许使用hack。hack应被视为最后的手段。在长期的项目中，允许使用hack只会带来更多的hack，你越是使用它，你越是会依赖它！
推荐使用：IE6使用下划线”_”，IE6、IE7使用星号“\*”,IE6/7/8/9使用“\\9”

### Css注意事项

-   页面内容里禁止夹杂CSS代码

-   不建议使用id作为标识

-   避免class与id重名

-   多个selector共用一个样式集，则多个selector必须写成多行形式

-   除了重置浏览器默认样式外，禁止直接为html tag添加css样式设置

-   不要轻易改动全站级CSS和通用CSS库。改动后，要经过全面测试

-   文章内页除文章容器可添加样式之外，请勿添加其他样式

-   避免使用CSS滤镜filter,如有特殊要求兼容ie6半透明的情况下可考虑使用

-   避免使用CSS表达式expression

-   尽量不要在CSS中使用!important

-   背景图片请尽可能使用css sprite技术, 减小http请求，可使用fis实现自动化合并。

## **图片规范**

### 命名

-   图片命名以有含义的英文或拼音命名。如：大背景图：bigbg.gif

-   采用主要含义[ + 次要含义
    ]的方式命名。(如：10x9的小图标icon_10x9.gif,登录按钮btn_login.gif)

-   遇到属性相同的图片，可以再加 \_01，或 \_a 区分。如:
    bg_100x50_01.gif，bg_100x50_a.gif

### 格式

-   大图片，且色彩较丰富，以高清为主的用jpg格式，图片品质一般60%，选择“连续”而非“优化”。

-   小图片，且表现简单的用gif格式。

-   透明或半透明图片用png格式。

-   非透明或半透明的图片禁止使用png格式。

### 体积大小

-   图片体积不能超过150KB（背景图除外）。

-   背景图过大，建议切成多张小图加载 。

-   jpg图片的品质一般选60%，选择“连续”而非“优化”。

-   png图片一定要进行压缩。

### 注意事项

-   所有图片标签必须加上alt属性，大图(尺寸宽或高超过100px)必须加上width,height属性。
    例如：\<img src="" width="500" height="200" alt="活动奖品建筑排程卡"/\>。

-   在保证视觉效果的情况下选择最小的图片格式与图片质量, 以减少加载时间。

-   使用css
    sprite技术合并小的背景图片，尽可能减少页面请求数。可使用相关工具进行自动合并，如fis。

-   能以背景形式呈现的图片，尽量写入css样式中。

-   禁止图片标签src属性为空，如暂无图片，可提供默认图片代替。

-   如有需要，一些图片可以做图片预加载。

-   关键模块以及变更比较频繁的图片尺寸保证为整10，或整5,如：150 \* 100。

## **流媒体规范**

-   前端不负责上传 mp4、f4v等视频文件。

### 注意事项

-   如特殊情况，需固定地址，可第一次由编辑上传后获取地址为固定地址（<http://192.168.10.50:8083/>[）；]()

### 特别注意事项

-   流媒体绝对不能放到www.tinytiger.cn服务器下，这个是禁忌！！！

## **SEO规范**

-   给重要的a标签加上title

-   目录式的链接，须在地址后面加上”/”，避免重定向，如:
    [http://xxx.tinytiger.cn/2014/jwtspk](http://xyq.163.com/2014/jwtspk)
    会进行一次302转向 应改为:
    [http://](http://xyq.163.com/2014/jwtspk/)[xxx.tinytiger.](http://xyq.163.com/2014/jwtspk)[cn/2014/jwtspk/](http://xyq.163.com/2014/jwtspk/)

-   根据超链接SEO权重优先级选用链接。链接SEO权重：域名\>目录\>文件

    -   如：[http://](http://xyq.163.com/2014/jwtspk/index.html)[xxx.tinytiger.](http://xyq.163.com/2014/jwtspk)[cn/2014/jwtspk/index.html](http://xyq.163.com/2014/jwtspk/index.html)

    -   改成[http://](http://xyq.163.com/2014/jwtspk/)[xxx.tinytiger.](http://xyq.163.com/2014/jwtspk)[cn/2014/jwtspk/](http://xyq.163.com/2014/jwtspk/)

-   超链接保持一致，不分散SEO权重

    -   如：[http://](http://zgmh.163.com/)[xxx.tinytiger.](http://xyq.163.com/2014/jwtspk)cn[/](http://zgmh.163.com/)
        [http://](http://zgmh.163.com/index.html)[xxx.tinytiger.](http://xyq.163.com/2014/jwtspk)[cn](http://xyq.163.com/2014/jwtspk/)[/index.html](http://zgmh.163.com/index.html)
        统一使用[http://](http://zgmh.163.com/)[xxx.tinytiger.](http://xyq.163.com/2014/jwtspk)[cn](http://xyq.163.com/2014/jwtspk/)[/](http://zgmh.163.com/)

-   链接形式的按钮，显示的文字，能用字体显示的，都必须用字体

-   导出链接(非公司站点的外部链接)需添加rel=”nofollow”属性

-   所有图片必须添加alt属性

-   单篇文章或页面的title 调用页面的主标题

-   专题或一个组合页面存在多个页面（不同url）的，必须每个页面的title
    都不一样,正常来说就是专题的其他页面也要做到title 调用页面的主标题

-   h1 h2 h3 标签用在页面最重要的关键词上

-   减少使用iframe

## **还原度规范**

### 基础要求

-   不存在样式错位和脚本报错

-   代码整洁工整，HTML 结构合理清晰，注释完善

-   表现与结构分离

-   避免重复造轮子，能用组件实现的一定要使用组件

-   各种间距与设计稿保持一致，如：行距、外边距、内边距、块与块之间距离

-   宽度、高度按照设计稿，切勿太随意

-   页面字体大小、颜色、是否加粗与设计稿一致

-   合并css小背景图标

### 浏览器兼容性

-   兼容chrome，IE10，Firefox浏览器。

### 屏幕适配

-   需兼容的屏幕分辨率包括：1024x768、1440x900、1680x1050、1920x1080

### 可读性

-   在屏蔽js、css后，页面要仍然具有良好的可读性。

-   flash无法加载的情况下，保证页面基本按钮或功能能使用。

### 注意事项

-   拖大和缩小窗口，页面布局不会发生错位。

-   flash在浏览器缩放情况下，能居中显示。

## **交互规范**

-   所有连接和按钮都有有hover前后交互状态。

-   文字链接必须有hover时交互状态，若设计稿无提供，给文字链接hover状态添加下划线示意。

-   同一个专题，或同风格页面之间的跳转，当前页打开。

-   不同产品，不同专题，不同页面风格之间跳转，可新窗口打开。

-   信息提示尽量避免alert系统弹层

-   分享组件都有hover状态

## **页面性能规范**

-   精简压缩JS,CSS

-   合并CSS背景小图标

-   优化压缩图片

-   避免空SRC的image

-   大图片添加width和height属性

-   避免缩放图片

-   减少操作DOM

-   减少视频，动画文件大小

-   页面总加载文件大小超过3M，必须找出原因解决，视频除外

-   页面运行cpu占用率超过40%，必须找出原因解决

-   尽量将JS放在页面底部引入

-   JS代码写在外部文件里引用

-   CSS放页面顶部

-   CSS写在外部文件里引用

-   减少使用CSS滤镜和CSS表达式

-   避免重定向

-   避免404

-   移除重复的脚本

-   减少使用iframe

-   优化favicon.ico，使其最小并且可缓存

### **关于测试**

-   页面制作完成后必须进行自测，兼容性测试，保证一定的出品质量。

-   提QA测试完成后，QA同事会出可修改测试文档，在确保每个修改完成条目后面
    用绿色文字 标明 已修改。确保每个QA条目都完成修改。

## **以下几个点为禁忌，绝对不得出错**

-   保密项目绝对不得因任何原因导致泄密

-   页面上绝对不得出现与本产品无关的其他产品信息（LOGO、ICON、产品名、文字、链接），宁可空白也不可以有，在自测时需确认正确。

-   与数字、金钱、活动有关的内容绝对不得出错，如：100w 写成了 10w；活动A的文案
    填充错成 活动B的文案。

-   超过100M的文件（包括但不限于：图片、音频、视频、压缩包、可执行文件、安装包等），需求方提供线上地址，前端不负责上传。
