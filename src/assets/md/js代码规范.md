

*   变量定义看具体情况而定，如使用ES6，要搭配babel使用；
*   多个变量定义用逗号分开，每个另起一行，逗号在最后
*   函数定义采用function开头定义（待定）
*   变量命名方式采用驼峰式，例如：nowPageNum
*   尽量避免使用全局变量，如需要使用，带双下划线前缀，首页字母小写，例如：`__cacheList`
*   全局模块、单例变量、公共组件等首写字母大写，例如：`PageController`
*   模块内全局变量，带下划线前缀，例如：`_cacheList`
*   模块内dom对象，带$符号前缀，例如：`$dom`
*   函数形参与内部私有变量，驼峰式就可以，例如：`pageIndex`
*   常量采用全大写，用\_分割，例如：`MAX_PAGE`

        /** 
        * @description 全局模块定义（采用闭包方式）
        * @method init 初始化
        * @example
        * PageControll.init();
        */
        var PageControll = function(){
            //模块内全局变量
            var _nowPageNum = 1;
            //dom对象
            var $pageDom = null;
            //常量
            var MAX_PAGE = 10;
    
            //初始化函数，也是对外接口
            function init(){
    
                //多个变量定义
                var temp1 = 1,
                    temp2 = 2;
    
                getPageList(1);
            }
    
            //私有函数，非对外
            /**
            * @description 拉取分页数据
            * @param {Number} pageNum 分页编号
            * @retrun null
            */
            function getPageList(pageNum){
    
                pageNum = pageNum || 1;
                //局部私有变量定义
                var pageFix = 5;
            }
    
            //返回外部接口，目前提供init
            return {
                init : init
            };
        }();
    

2、格式规范：
-------

*   代码注释采用jsdoc方式，需注明代码的依赖
*   代码缩进采用4个空格方式
*   操作符前后保留1个空格，例如：`1 + 1 = 2`;
*   每行代码结束添加分号
*   同功能代码写在一个区域，变量定义统一在顶部，例如

        var _mod = 1,
            _mod2 = 3;
        function getList(){}
        function getPage(){}
    

各种类型定义使用，如下：

        var num = 1;
        var str = 'test';
        var array = [];
        var json = {};
    

if else语句使用，if只有一句代码也需要{}

    if(条件1 || 条件2){
        //执行1
    }
    else {
        //执行条件2
    }
    

for语句使用

    for(var i = 0,l = list.length;i < l;i++){
        //执行语句，list[i]
    }
    for(var key in jsonObj){
        //执行语句，jsonObj[key]
    }
    

while语句使用

    var i = 0,
        l = list.length;
    while(i < l){
        //执行语句
        i++;
        //注意死循环
    }
    

do while语句，尽量避免使用，采用for或者while代替 switch语句使用，`当if else达到3个层级以上用swtich`

    switch(index){
        case 1:
            //do something
            break;
        case 2:
            //do something
            break;
        default:break;
    }
    

try catch语句，与if else类似 尽量避免使用continue、with、goto、eval、new Function等业界冷门或者有诟病语句

3、代码规范：
-------

setTimeout，setInterval方法需要传方法引用，禁止传字符串 计时器时间间隔，需要`大于100ms`（做动画除外），小于100ms，用户无法感知，以及耗资源 多元操作符使用

    //容错、赋默认值，注意容错值是否可以false，0，''等值
    var event = window.event || e;
    //函数执行
    isName && Test();
    //与下面语句意思一样，建议使用以下这种
    if(isName){
        Test();
    }
    //三元操作符
    var val = isName ? 'name' : 'error';
    //不建议三元层级关系太多，容易混乱以及可读性差，例如
    var val = isName ? 'name' : isChines ? 'china' : 'error';
    

对于`超过1次使用的dom或者bom对象`，需要用变量缓存起来。例如：

    var dom = document.getElemntById("test");
    dom.style.left = '1px';
    dom.style.top = '2px';
    

回调中包含另外回调（匿名函数），尽量避免，影响可读性，例如：

    $.ajax(function(){
    
        $.post(function(){
            //do something
        });
    });
    

多重循环，尽量避免，例如：

    for(var i = 0,l = 10;i < l;i++){
        for(var j = 0;k = 10;j < k;j++){
            //do something
        }
    }
    

禁止直接用onxxx绑定事件，例如：

    document.onkeydown = function(e){
        //do something
    }
    

全局情况下，判断变量是否为存在，例如：

    //错误的使用
    if(foo)return true;
    //正确的使用
    if(window.foo)return true;
    if(typeof foo !== "undefined")return true;
    

4、文件规范：
-------

文件名与文件内的模块名保持一致，字母全小写，用-分割，例如：`mod-index.js` 同页面或者同大模块的文件，放在一个独立文件夹中，例如： index文件夹 |--mod-index.js |--db-index.js css文件放head中，js文件放body底部，除非需要延迟加载

    <head>
        <link rel="stylesheet" href="css/style.css" />
    </head>
    <body>
        /**body内容代码*/
        <script type="text/javascript" src="js/index.js"></script>
    </body>
    

尽量避免内嵌css、js代码在html页面中 html标签中，class与id，都需要带上js\_前缀，表示这个是提供给js用，并没有任何样式上的关联

    <div class="head js_head"></div>
    <div id="js_head"></div>
    

5、关于性能
------

> JS性能优化原则
> 
> *   减少改变dom结构的次数
> *   减少选择器的层数，尽量使用id,或带id的选择器，如使用`$("#ul_test li")`而不要使用`$(".ul_test li")`
> *   注意内存回收，特别是将dom节点或dom节点绑定的方法赋值给某个变量，要注意内容回收
> *   使用面向对象开发模式，减少变量污染
> *   减少绑定方法的次数
> *   资源文件载入最小化，如选择合适的图片格式，减少图片大小，对js及css进行压缩等。

### 1\. 大循环体性能优化：

*   for循环体要先将循环次数放到一个变量中，避名大循环体下多次取值影响性能：

    //错误的写法
        for(var i = 0; i < m.length; i++){
    
        }
    //正确的写法
        for(var i=0,len=m.length; i<len; i++){
    
        }
    

*   如果循环体每一次循环都需要多次对dom结构添加元素，先使用变量保存所有要添加的元素HTML或临时DOM节点，在循环完后`一次性修改dom结构`，避免每次修改dom结构时浏览器对整个dom结构进行重新解析，从而影响性能

        //错误的写法
        for(var i = 0, len = m.length; i < len; i++){ 
            $("#aaa").append("<p>123123</p>");
        }
        //正确的写法
        var html="";
        for(var i = 0,len = m.length; i < len; i++){ 
            html+="<p>123123</p>";
        }
        $("#aaa").append(html);
    ``````
    

### 2.方法绑定

使用on()或delegate()代替bind()来为节点绑定方法

> 使用bind绑定方法经常会面临几个常见问题：
> 
> *   当dom节点发生变化时，绑定事件失效，
> *   每次修改dom节点都要重新绑定事件
> *   使用ajax之类添加的节点，只能在ajax回调之后绑定，当重复调用ajax时会面对上一个问题
> 
> 使用on()或delegate()的实现事件代理 使用 delegate()和on()方法的事件处理程序适用于当前或未来的元素（比如由脚本创建的新元素）,当元素节点发生变化时无需重新绑定

        $(document).on("click","#ul_list li",function(e){
    
        });
        $("#test").delegate("li","click",,function(e){
    
        });
    

### 3.js面向对象开发：

> 在开发过程中尽量使用面向对象开发模式
> 
> *   单例模示：对各个子功能进行模块封装，实现模块化开发，减少全局变量的使用，方便代码管理及模块修改，使用闭包灵活改变公有或私有变量的生命周期。
> *   类：对可重用插件进行封装,方便拓展及重用；

单例结构示例：

    var Artical=function(window,document){
        var _articalData,
        _totalPages=1,//私有全局变量
        _curPages=1,
         var init=function(param){//私有函数，使用这种方法定义函数需要保证其本身被调用时必须在此函数声明之后
            _nextHost="";
            _totalPages=1;
            _curPage=1;
             getPage();
             addEvent();
         }
         function getPage(){//私有函数
    
             if(_articalData){
                 formatArtical(_articalData);
             }
             else{
                 var localparam=location.href.split("/");
                 var urllink=unescape(localparam[localparam.length-1].split("-")[1]);
                 $.ajax({
                    url:_webHost+urllink,
                    dataType:"html",
                    type:"get",
                    success:function(data){
                        formatArtical($(data));
                    }
                })
            }
         }
         function getMore(nextUrl){
    
         }
         var addEvent=function(){
    
         }
         var remove=function(){
             _articalData=null;
         }
    
         var exports = {//开放给外部调用的函数
            init : init,
            remove:remove,
            setData:function(data){//提供给外部修改内部变量的方法
                _articalData=data;
            }
        };
        return exports;
    
     }(window,document,undefined);
     //调用示例
     Artical.init();//调用初始化函数
     Artical.setData("111");//修改内部变量_articalData
    

类结构示例（采用单例和原型链结合），减少实例化时使用new 实例化对象

    var Mobile = function(){ //定义单例
        var _styleLink=null;//所有对象实例公有参数
        var setParam=function(options){//所有对象实例公有函数
        }
        var _class = function(options){//内部类
            var classParam="test";//对象私有变量
            this.options=$.extend({
                param1:null,
                param2:null,
                callback:null
            },options)
            this.init();
        }
        _class.prototype = {//对象私有方法
            init : function(){//对象初始化
                 this.addEvent();
    
            },
            addEvent:function(){//事件绑定
    
            }
        }
        return {
            setting:function(options){//提供给外部调用的方法，修改所有实例的公有参数
                 setParam(options);
            },
            create : function(options){//提供给外部调用的方法，创建实例
                return new _class(options);
            }
        }
    }();
    
    //调用实例
    Mobile.setting({//设置对象所有实例的公有参数
                param1:"1111",
                param2:"2222"
             })
    var mb = Mobile.create({//创建实例1
        param1:"aaa",
        param2:"bbb",
        callback:function(){}
    });
    var mb2= Mobile.create({//创建实例2
        param1:"ccc",
        param2:"ddd",
        callback:function(){}
    });
    

### 4.避免使用全局变量以及在DOM文档中重复对相同节点查找

*   使用全局变量的开销比使用局部变量的开销大。
*   重复对同一个节点进行dom文档搜索对浏览器消耗也很大，应在第一次查找时将结果保存在一个变量后，后面重复使用时调用变量；

        var $li_tags=$("#ul_test li");
        for(var i= 0; len = li_tags.length; i < len ;i++){
    
        }
        $li_tags.find("a").remove();
    

### 5\. 内存使用及回收

*   将节点赋值给变量时，如果id为ul\_test的节点一直保留在dom文档中，那这个li\_tags变量不会被浏览器自动回收而一直存在内存中，所以当li\_tags使用完后用li\_tags=null，浏览器才会自动回收li\_tags。例如

        var $li_tags=$("#ul_test li");
        $li_tags.each(function(index,ele){
            //do something.....
        });
        $li_tags=null;
    

*   当使用闭包时，也要注意内存的回收，一旦使用了闭包，闭包变量因为同时有多个函数在使用，也不会被自动回收，有需要回收需要手动回收；
*   移动端开发时，ios对内存分配管理比较严格，而且是有内存上限的，当内存溢出时会造成程序闪退，尤其要注意大数据存放在内存的问题，若仅一次使用要将变量置为null,若会被多次调用可考虑存到localstorage中
*   移动端开发时，还要注意单次循环的次数不宜过多，当js长期处于循环处理而不响应时，浏览器也有可能会闪退，这种现象主要存在于低端ios机型,如确实需要大量循环，可使用递归分块处理