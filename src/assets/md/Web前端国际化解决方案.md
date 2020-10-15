# Web前端国际化解决方案

## **国际化简介**

国际化（Internationalization，简写为i18n）是指网站能用于多国语言环境的能力。网站国际化一般都有切换语言的按钮，供不同国家的用户使用。随着公司的发展，未来游戏海外发行量将逐渐上升，网站国际化业务随着增多，因此迫切需要解决网站前端国际化的问题。

#### 过去我们是这样去解决国际化问题的：

  
一个网站有两种语言切换，如[简体中文\|English]。我们是写成两套布局，然后利用display去显示一套，隐藏另外一套。点击切换语言后便交叉显示或隐藏。  
这样做的缺点：

-   开发效率低 ：需要写几套语言的布局

-   代码冗余大 ：存在大量重复性的代码

-   后期维护难 ：后期想修改代码会比较麻烦

如果有4、5套语言切换呢？还按照这种方法吗？显然不是的\~

#### 现在我们是这样来解决前端国际化问题的：

原材料  
*jquery(mixNIE).last.js*  
[underscore.js](http://res.nie.netease.com/comm/i18n/js/underscore.js)  
[backbone.js](http://res.nie.netease.com/comm/i18n/js/backbone.js)  
具体方法  
采用MVC的方法将数据与表现相分离，这里采用了*backbone.js*,它依赖*underscore.js*，所以需要在*backbone.js*之前引入*underscore.js*。
*backbone.js*正是一个优秀的JSMVC框架，它提供了3个东西：models(模型)；views(视图)；collections(集合)
。  
Backbone.Model 表示应用中所有数据  
Backbone.View
是可以绑定页面的DOM，页面的HTML就是通过View中的render()方法渲染出来的，通常一个View会有一个Model作为数据  
Backbone.Collection
其实就是一个保存Models的集合类，具有增加元素、删除元素、排序等方法

首先，定义不同国家语言的JSON数据  
其次，利用*underscore.js*中的模板引擎，在网页中书写template，语法：<p
class="brief"\>\<%= brief %\><p\>  
再次，在JS中定义一个控制语言的变量，当然移动端的网站，还是建议使用HTML5中的localStorage来控制  
最后，根据不同的语言，异步请求不同的JSON数据，渲染页面  
这样做的优点：

-   开发效率高 ：一个布局，通过请求不同的数据，渲染出不同的页面

-   代码精简 ：只存在一套模板代码

-   易于维护 ：MVC将数据与表现相分离，方便后期维护

## **代码实现**

#### 源码结构说明

```

src --  //源码路径
|--js
|--css
|--images
|--nls --
|  |--zh-cn --
|  |  |--data.json  //简体中文
|  |--en-us --
|  |  |--data.json  //English
|--index.html
README.md -- //说明文档
```

#### 不同国家语言的JSON数据

简体中文

```
{

"result": {

"language": "zh-cn",

"language_desc": "English",

"brief": "大家好，这是国际化的例子\~",

"red": "红色",

"orange": "橙色",

"yellow": "黄色",

"green": "绿色",

"cyan": "青色",

"blue": "蓝色",

"purple": "紫色",

"imgurl": "/comm/i18n/images/tinytigergames_zh-cn.png"

}

}

English

{

"result": {

"language": "en-us",

"language_desc": "中文简体",

"brief": "Hi all,this is the i18n demo\~",

"red": "red",

"orange": "orange",

"yellow": "yellow",

"green": "green",

"cyan": "cyan",

"blue": "blue",

"purple": "purple",

"imgurl": "/comm/i18n/images/tinytiger_en-us.png"

}

}
```

附：国际化中部分国家命名的简写

| **国家**           | **简写** |
|--------------------|----------|
| 简体中文(中国)     | zh-cn    |
| 繁体中文(台湾地区) | zh-tw    |
| 繁体中文(香港)     | zh-hk    |
| 英语(香港)         | en-hk    |
| 英语(美国)         | en-us    |
| 英语(英国)         | en-gb    |
| 英语(加拿大)       | en-ca    |
| 英语(澳大利亚)     | en-au    |
| 英语(新加坡)       | en-sg    |
| 英语(泰国)         | en-th    |
| 英语(马来西亚)     | en-my    |
| 韩文(韩国)         | ko-kr    |
| 日语(日本)         | ja-jp    |
| 法语(法国)         | fr-fr    |
| 德语(德国)         | de-de    |

## **书写模板**

```
<!-- template -->
    <script type="text/html" id="template">
      <p class="brief"><%=brief%></p>
      <section class="color red"><%=red%></section>
      <section class="color orange"><%=orange%></section>
      <section class="color yellow"><%=yellow%></section>
      <section class="color green"><%=green%></section>
      <section class="color cyan"><%=cyan%></section>
      <section class="color blue"><%=blue%></section>
      <section class="color purple"><%=purple%></section>
      <section class="img png24" data-pic=<%=imgurl%>></section>
    </script>
<!-- template/-->
```

## **书写脚本**

/**
 * gzsuqiuhong on 20141119
 * i18n demo
 **/
var pageCtrl = {
    //定义初始语言
    lang: 'zh-cn',
    //请求数据
    requestData: function(lang) {
        $("#content").html("").append('<section class="loading"></section>');
        setTimeout(function() {
            $.getJSON('../i18n/nls/' + lang + '/data.json', function(data) {
                if (!jQuery.isEmptyObject(data.result)) {
                    var View = Backbone.View.extend({
                        el: '#content',
                        template: $("#template").html(),
                        //页面渲染
                        render: function() {
                            var temp = _.template(this.template, data.result);
                            this.$el.html(temp);
                            //设置section.img的背景图
                            var imgurl = $(".img").attr("data-pic");
                            $(".img").css({
                                backgroundImage: 'url(' + imgurl + ')'
                            });
                        },
                        //页面初始化
                        initialize: function(options) {
                            this.render();
                        }
                    });
                    //实例化对象
                    var View = new View();
                } else {
                    alert("数据为空，页面渲染失败！");
                }
            });
        }, 600);
    },
    //页面初始化
    init: function() {
        i18n = this;
        i18n.requestData(i18n.lang);
        $(".lang").click(function(event) {
            var _lang = $(this).attr("lang");
            if (i18n.lang != _lang) {
                pageCtrl.lang = _lang;
                i18n.requestData(i18n.lang);
            }
            return;
        });
    }
}
pageCtrl.init();
```