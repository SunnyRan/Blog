---
title: HTML优化之按需加载
date: 2016-05-20 15:15:24
tags: CSDN迁移
---
  按需加载是前端性能优化中的一项重要措施，按需加载是如何定义的呢？顾名思义，指的是当用户触发了动作时才加载对应的功能。触发的动作，是要看具体的业务场景而言，包括但不限于以下几个情况：鼠标点击、输入文字、拉动滚动条，鼠标移动、窗口大小更改等。加载的文件，可以是JS、图片、CSS、HTML等。后面将会详细介绍“按需”的理解。   
 按需解析HTML   
 按需解析HTML，就是页面一开始不解析HTML，根据需要来解析HTML。解析HTML都是需要一定时间，特别是HTML中包含有img标签、引用了背景图片时，如果一开始就解析，那么势必会增加请求数。常见的有对话框、拉菜单、多标签的内容展示等，这些一开始是不需要解析，可以按需解析。实现按需解析，首先用

 
```
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>按需解析HTML</title>
</head>
<body>
<script type="text/x-template" id="suc_subscription">
    <!--假设这里的样式box-dytz 引用了一张背景图--->
    <div>
    <!--这里暂且用这张图片作为测试，实际中，大家可以替换为任何图片-->
    <img src="http://tid.tenpay.com/wp-content/uploads/2012/12/按需加载.jpg" />
    </div>
</script>
<div id="success_dilog"></div>
<input type="button" value="点我展示HTML"  onclick="showHTML()"  />
<script>
    function showHTML(){
        document.getElementById('success_dilog').innerHTML =  document.getElementById('suc_subscription').innerHTML;
    }
</script>
</body>
</html>
```
 我们一起来看下demo，当运行demo并抓包发现：当页面加载结束时，并没有看到图片的请求；当点“点我展示HTML”按钮时，通过抓包发现有图片请求。   
 曾经做个demo并经过测试发现，如果是直接解析HTML（不包含有请求CSS图片和img标签），耗费的时间要比用大约慢1-2倍，如果是还包括请求有CSS图片、img标签，请求连接数将会更多，可见按需解析HTML，对性能提升还是有一定效果。   
 按需加载图片   
 按需加载图片，就是让图片默认开始不加载，而且在接近可视区域范围时，再进行加载。也称之为懒惰加载。大家都知道，图片一下子全部都加载，请求的次数将会增加，势必影响性能。   
 先来看下懒惰加载的实现原理。它的触发动作是：当滚动条拉动到某个位置时，即将进入可视范围的图片需要加载。实现的过程分为下面几个步骤：   
 生成![]()标签时，用data-src来保存图片地址；   
 记录的图片data-src都保存到数组里；   
 对滚动条进行事件绑定,假设绑定的函数为function lazyload(){};   
 在函数lazyload中，按照下面思路实现：计算图片的Y坐标，并计算可视区域的高度height，当Y小于等于(height+ scrollTop)时，图片的src的值用data-src的来替换，从而来实现图片的按需加载；   
 下面看一个示例代码：

 
```
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>图片按需加载</title>
</head>
<body>
    <style>
        li {float:left;width:200px;}
    </style>
    <div style="widh:100%;height:1200px;border:1px solid #000">这里是空白的内容，高度为900像素，目的是方便出现滚动条</div>
    <ul style="width:600px;">
        <li> <img width="158" height="158" data-src="http://pic6.nipic.com/20100423/4537140_133035029164_2.jpg"  /> </li>
        <li> <img width="158" height="158" data-src="http://www.jiujiuba.com/xxpict2/picnews/62223245.jpg"  /> </li>
        <li> <img width="158" height="158" data-src="http://www.bz55.com/uploads/allimg/100729/095KS455-0.jpg"  /> </li>
        <li> <img width="158" height="158" data-src="http://www.hyrc.cn/upfile/3/200611/1123539053c7e.jpg"/> </li>
        <li> <img width="158" height="158" data-src="http://www.mjjq.com/blog/photos/Image/mjjq-photos-903.jpg" /> </li>
        <li> <img width="158" height="158" data-src="http://c.cncnimg.cn/000/954/1416_2_b.jpg"  /> </li>
        <li> <img width="158" height="158" data-src="http://www.jiujiuba.com/xxpict2/picnews/62223231.jpg" /> </li>
        <li> <img width="158" height="158" data-src="http://www.mjjq.com/pic/20070530/20070530043314558.jpg" /> </li>
    </ul>
    <script>
        var API = {   
            /**
              * 兼容Mozilla(attachEvent)和IE(addEventListener)的on事件
              * @param {String} element DOM对象 例如：window，li等
              * @param {String} type on事件类型，例如：onclick，onscroll等
              * @param {Function} handler 回调事件
              */
            on: function(element, type, handler) {
                return element.addEventListener ? element.addEventListener(type, handler, false) : element.attachEvent('on' + type, handler)
            },   
            /**
              * 为对象绑定事件
              * @param {Object} object 对象
              * @param {Function} handler 回调事件
              */
            bind: function(object, handler) {
                return function() {
                    return handler.apply(object, arguments)
                }
            },   
            /**
              * 元素在页面中X轴的位置
              * @param {String} element DOM对象 例如：window，li等
              */
            pageX: function(El) {
                var left = 0;
                 do {
                    left += El.offsetLeft;

                } while(El.offsetParent && (El = El.offsetParent).nodeName.toUpperCase() != 'BODY');
                return left;

            },   
            /**
              * 元素在页面中Y轴的位置
              * @param {String} element DOM对象 例如：window，li等
              */
            pageY: function(El) {
                var top = 0;
                 do {
                    top += El.offsetTop;

                } while(El.offsetParent && (El = El.offsetParent).nodeName.toUpperCase() != 'BODY');
                return top;

            },   
            /**
              * 判断图片是否已加载
              * @param {String} element DOM对象 例如：window，li等
              * @param {String} className 样式名称
              * @return {Boolean} 布尔值
              */
            hasClass: function(element, className) {
                return new RegExp('(^|\\s)' + className + '(\\s|$)').test(element.className)
            },   
            /**
              * 获取或者设置当前元素的属性值
              * @param {String} element DOM对象 例如：window，li等
              * @param {String} attr 属性
              * @param {String} (value) 属性的值，此参数如果没有那么就是获取属性值，此参数如果存在那么就是设置属性值
              */
            attr: function(element, attr, value) {
                 if (arguments.length == 2) {
                    return element.attributes[attr] ? element.attributes[attr].nodeValue : undefined
                }
                else if (arguments.length == 3) {
                    element.setAttribute(attr, value)
                }
            }
        };

        /**
          * 按需加载
          * @param {String} obj 图片区域元素ID
          */
        function lazyload(obj) {
            this.lazy = typeof obj === 'string' ? document.getElementById(obj) : document.getElementsByTagName('body')[0];
            this.aImg = this.lazy.getElementsByTagName('img');
            this.fnLoad = API.bind(this, this.load);
            this.load();
            API.on(window, 'scroll', this.fnLoad);
            API.on(window, 'resize', this.fnLoad);

        }
        lazyload.prototype = {

            /**
              * 执行按需加载图片，并将已加载的图片标记为已加载
              * @return 无
              */
            load: function() {
                var iScrollTop = document.documentElement.scrollTop || document.body.scrollTop;
                // 屏幕上边缘
                var iClientHeight = document.documentElement.clientHeight + iScrollTop;
                // 屏幕下边缘
                var i = 0;
                var aParent = [];
                var oParent = null;
                var iTop = 0;
                var iBottom = 0;
                var aNotLoaded = this.loaded(0);
                 if (this.loaded(1).length != this.aImg.length) {
                    var notLoadedLen = aNotLoaded.length;
                     for (i = 0; i < notLoadedLen; i++) {
                        iTop = API.pageY(aNotLoaded[i]) - 200;
                        iBottom = API.pageY(aNotLoaded[i]) + aNotLoaded[i].offsetHeight + 200;
                        var isTopArea = (iTop > iScrollTop && iTop < iClientHeight) ? true : false;
                        var isBottomArea = (iBottom > iScrollTop && iBottom < iClientHeight) ? true : false;
                         if (isTopArea || isBottomArea) {                   
                            // 把预存在自定义属性中的真实图片地址赋给src
                            aNotLoaded[i].src = API.attr(aNotLoaded[i], 'data-src') || aNotLoaded[i].src;
                             if (!API.hasClass(aNotLoaded[i], 'loaded')) {
                                 if ('' != aNotLoaded[i].className) {
                                    aNotLoaded[i].className = aNotLoaded[i].className.concat(" loaded");

                                }
                                else {
                                    aNotLoaded[i].className = 'loaded';

                                }
                            }
                        }
                    }
                }
            },

            /**
              * 已加载或者未加载的图片数组
              * @param {Number} status 图片是否已加载的状态，0代表未加载，1代表已加载
              * @return Array 返回已加载或者未加载的图片数组
              */
            loaded: function(status) {
                var array = [];
                var i = 0;
                 for (i = 0; i < this.aImg.length; i++) {
                    var hasClass = API.hasClass(this.aImg[i], 'loaded');
                     if (!status) {
                        if (!hasClass)
                            array.push(this.aImg[i])
                    }
                    if (status) {
                        if (hasClass)
                            array.push(this.aImg[i])
                    }
                }
                return array;       
            }
        };
        // 按需加载初始化
        API.on(window, 'load', function () {new lazyload()});
    </script>
</body>
</html>
```
 运行上述的示例代码，并抓包会发现：一开始并没有看到图片的请求，但当拉动滚动条到页面下面时，将会看到图片发送请求。目前很多框架都已经支持图片的懒惰加载，平时在开发中，大家可以对图片实现懒惰加载，这是有效提升性能的一个方法，特别是网页图片比较多时，更加应该使用该方法。   
 按需加载除了上述场景外，还有更多的场景。如下图：   
 ![这里写图片描述](https://img-blog.csdn.net/20160520151457279)   
 页面一开始，加载的是“全部”标签里面的内容，但在点击“指定商品折扣券”标签时，才去加载对应的图片。实现思路如下：   
 生成![]()标签时，用data-src来保存图片地址；   
 在点击标签事件时，获取所有图片，图片的src的值用data-src的来替换；   
 示例代码如下：

 
```
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>标签按需加载</title>
</head>

<body>
    <style>
        ul li {width:200px;height:30px;float:left; list-style:none; text-align:center;}
        .on{border-top:1px solid #3FF;border-left:1px solid #3FF;border-right:1px solid #3FF; }
        .hide{display:none;}
    </style>
    <ul>
        <li>全部</li>
        <li id="viewTsb1" onclick="showTabContent()">指定商品折扣券</li>
    </ul>
    <div style="width:800px;height:500px;clear:both;">
        <div id="tab1" style="height:25px; line-height:25px; margin:50px 0 0 40px">全部标签应该展示所有内容</div>
        <div id="tab2">
             <img width="158" height="158" data-src="http://img2.114msn.com/jindian/20081071153761308.jpg"  />
             <img width="158" height="158" data-src="http://www.mjjq.com/blog/photos/Image/mjjq-photos-900.jpg"  />
         </div>
    </div>
    <script>
        var isLoadedImg = false;
        function showTabContent(){ 
            if(isLoadedImg){
                return;
            }
            var elem = document.getElementById("tab2");
            document.getElementById("tab1").className="hide";
            elem.className="";
            var arrImage = elem.getElementsByTagName("img");
            var l = arrImage.length;
            for(var i=0;i<l;i++){
                arrImage[i].src = arrImage[i].getAttribute('data-src');
            }
            isLoadedImg = true;
            //todo 更改标签状态
        }
    </script>
</body>
</html>
```
 运行上述代码并抓包并发现：一开始没有看到有图片的请求，但点击“指定商品折扣券”标签时，看到有图片的请求发送。需要注意的是，为了确保体验，首屏的图片不建议懒惰加载，而应该直接展示出来；避免一开始用户就无法看到图片，在IE下看到一个虚线框，这样体验反而不好。   
 按需执行JS   
 按需执行JS和懒惰加载图片比较类似。当打开网页时，如果等所有JS都加载并执行完毕，再把界面呈现给用户，这样整体上性能会比较慢，体验也不友好。就是当某个动作触发后，再执行相应的JS，以便来渲染界面。按需执行JS，可以应用在下列场景：执行一些耗时比较久的JS代码，或执行JS后，需要加载比较多图片、加载iframe、加载广告等。在一些webapp的应用中，或比较复杂的页面时，更加应该使用这种方法。   
 实现思路和按需加载比较类似：   
 对滚动条进行事件绑定,假设绑定的函数为function lazyExecuteJS(){};   
 在函数lazyExecuteJS中，按照下面思路实现：选择一个元素作为参照物，当滚动条即将靠近时该元素位置，开始执行对应的JS，从而实现对界面的渲染；   
 示例代码如下（以YUI3框架为例）：   
 首先下载最近封装的异步滚动条加载组件：Y.asyncScrollLoader，然后运行下面的代码（需要把页面和Y.asyncScrollLoader.js 放在同一个目录）：

 
```
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>按需执行JS</title>
</head>
<script src="http://yui.yahooapis.com/3.2.0/build/yui/yui-debug.js"></script>
<body>
    <div style="height:1500px"><span style="">向下拖动滚动条，拉到广告条的位置，将会自动加载广告的内容！</span></div>
    <div id="ADContent">这里是广告的内容，将会用JS进行填充</div>
    <script>
        YUI({modules:{'asyncScrollLoader': { 
            fullpath: 'Y.asyncScrollLoader.js', 
                type: 'js', 
                requires:['widget'] 
            } 
        }}).use('widget','asyncScrollLoader', "node", function(Y){
            var  loadAD = function(){
                //这里可以用简单的代码代替，实际项目中，可以执行任何JS代码，如请求CGI，或者广告数据，然后再填充到页面上
                var html = '<div><img src="http://t2.baidu.com/it/u=2027994158,3223530939&fm=23&gp=0.jpg" alt="" /><span>哈哈，我也是动态加载进来的，可以从html结构或抓包看出\效果哈！</span><\/div>'
                Y.one('#ADContent').set('innerHTML',html);
            }

            var cfg = { 
                'elementName' : 'div', 
                'className'     : 'lazy-load', 
                'contentAttribute' : '' ,
                'foldDistance': 10,
                'obCallback' : {
                    'funName' : loadAD,
                    'argument' : [],
                    'context' : this
                }
            };

           new Y.asyncScrollLoader(cfg).renderer(); 
        });      
    </script>
</body>
</html>
```
 运行上述代码并抓包发现：打开页面时，是不没有看到有对应的图片请求，但当滚动条拉到一定位置时，loadAD的函数被执行。   
 按需加载JS   
 JavaScript无非就是script标签引入页面，但当项目越来越大的时候，单页面引入N个js显然不行，合并为单个文件减少了请求数，但请求的文件体积却很大。这时候比较合理的做法就是按需加载。按需加载和按需执行JS比较类似，只不过要执行的JS变成了固定的“实现加载JS”的代码。按需加载实现的思路如下：   
 对滚动条进行事件绑定,假设绑定的函数为function lazyLoadJS(){};   
 在函数lazyLoadJS中，按照下面思路实现：选择一个元素作为参照物，当滚动条即将靠近时该元素位置，开始执行加载对应JS；   
 在JS加载完毕后，开始执行相应的函数来渲染界面；   
 在实际项目中，可以根据需要设置一个目标距离，比如还有200像素该元素即将进入可视区域；按需加载JS和按需执行JS比较类似，这里就不再单独提供示例代码了；大家可以在按需执行JS的中示例中，把loadAD函数更改为动态加载JS即可；   
 分屏展示   
 当一个网页比较长，有好几个屏幕，而且加载了大量的图片、广告等资源文件时，分屏展示，可提升页面性能和用户体验。其实分屏展示也可以从按需加载的的角度来看待，默认是加载第一屏幕的内容，当滚动条拉动即将到达下一个屏幕时，再开始渲染下个屏的内容。换言之，是把图片、背景图片、HTML一起按需加载，一开始不对HTML进行解析，那么背景图、img图片也不会进行加载。   
 分屏展示的思路如下：   
 根据具体业务情况，收集主流最大的分辨率的高度；假设这里是用960px;   
 按照这个高度进行分屏，依次把下一个屏幕内的HTML用HTML

 
```
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>网页分屏展示</title>
</head>
<style>
    .class2 {
        visibility:hidden;
        widh:100%;
        height:300px;
    }
</style>
<script src="http://yui.yahooapis.com/3.2.0/build/yui/yui-debug.js"></script>
<body>
<div style="height:1500px"><span style="">向下拖动滚动条，美图在下面！</span></div>
<textarea>
    <div>
        <img src="http://travel.shangdu.com/liu/uploads/allimg/090407/1521221.jpg" alt="" /><span style="font-size:16px;color:red;">我是动态加载进来的，可以从html结构或抓包看出效果哈！</span>
    </div>
</textarea>
<div style="height:800px"><span style="">下面继续上美图！</span></div>
<textarea>
    <div>
        <img src="http://download.99sucai.com/upload/images/201006042009245.jpg"  alt="" /><span>哈哈，我也是动态加载进来的，可以从html结构或抓包看出效果哈！</span>
    </div>
</textarea>
<script>
 YUI({modules:{'asyncScrollLoader': { 
     fullpath: 'Y.asyncScrollLoader.js', 
     type: 'js', 
     requires:['widget'] 
     } 
 }}).use('widget','asyncScrollLoader', "node", function(Y){
     var cfg = { 
        'elementName' : 'textarea', 
        'className'     : 'class2', 
        'contentAttribute' : 'value' ,
        'foldDistance': 10           
    };        
    new Y.asyncScrollLoader(cfg).renderer(); 
 });      
</script>
</body>
</html>
```
 运行上面代码并抓包发现：在默认首屏，并没有去解析textarea里面的代码，但当拉动滚动条到一定位置时，textarea里面的HTML依次被解析，从而实现了网页分屏展示。上述的示例代码，可以在这里下载来查看： 示例代码   
 使用“按需加载”进行性能优化时，需要合理选择触发的动作。“按需加载”的最大优势在于减少了不必要的资源请求，节省流量，真正实现“按需所取”。但是“按需加载”本身如果使用不当也会影响用户体验，因为“按需加载”的时机在用户触发某动作之后，如果用户的网速比较慢的话，加载脚本或执行脚本可能需要等候较长的时间，而用户则不得不为此付出代价。因此，如果要使用“按需加载”则需要选择正确的触发动作，如果是根据滚动条来触发，可考虑一个目标距离，假设目标距离还有200像素即将进入可视区域，则就开始加载，而不是等到进入了可视区域才加载。以上所讲的各种“按需加载”类型，都可以封装成相应的组件，然后就可以在项目中进行应用。

   
  