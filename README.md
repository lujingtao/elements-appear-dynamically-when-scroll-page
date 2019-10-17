@[toc](js+animate.css实现滚动页面，元素动态出现（附演示地址和源码）)

# 一、背景
之前做项目用到滚动页面，元素动态出现的效果，当时研究一番决定用Animate.css + fullpage.js + 一堆js控制，这段时间也有项目也有这种需求，不过不是整页滚动，而是不确定高度滚动，所有用不到fullpage，于是只能手写js控制。

# 二、效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191017104701564.gif)

# 三、原理
整个动画思路关键点就是：

- 1、理解Animate.css的使用（其实就是加类名）
- 2、需要动画的元素加类名“animated ”和“消失效果类名”，例如：“animated bounceOutDown”
- 3、js检测滚动，当滚动到区块的时候，去掉“消失效果”，再添加“对应的呈现效果”，例如：“animated bounceInDown”
- 4、再加一些细节问题，例如 动画的元素默认css设置为visibility: hidden，显示时再visibility: visible；

# 四、在线演示和源码
[--> 在线演示地址](https://lujingtao.github.io/elements-appear-dynamically-when-scroll-page/demo/)
[--> GitHub源码](https://github.com/lujingtao/elements-appear-dynamically-when-scroll-page)


核心代码：
```js
//上下滚动事件
(function($) {

    var guide = $(".sideFixed");
    var guideLi = $(".sideFixed .t");
    var index = 0;
    var page0End = false;
    var page1End = false;
    var page2End = false;
    var page3End = false;


    //屏幕滚动
    var goToFun = function() {
        console.log(index);
        var top = index >= guideLi.size() - 1 ? 0 : $(".column" + index).offset().top;
        $("html,body").stop().animate({ scrollTop: top }, 600, "swing", function() {
            scrollFunc();
        });
    }

    guideLi.each(function(i) { $(this).click(function() { index = guideLi.index($(this));
            goToFun(); }) });

    /* 滚轮事件 */
    var scrollFunc = function(e) {
        e = e || window.event;
        //区块一
        if ($(document).scrollTop() >= $(".column0").offset().top - 400 && !page0End) {
            $("#area0-1").removeClass('bounceOutDown').css("visibility","visible").addClass("bounceInDown");
            setTimeout(function() { $('#area0-2').removeClass('bounceOutDown').css("visibility","visible").addClass("bounceInDown"); }, 200);
            page0End = true;
        }
        //区块二
        if ($(document).scrollTop() >= $(".column1").offset().top - 400 && !page1End) {
            $("#area1-1").removeClass('zoomOut').css("visibility","visible").addClass("zoomIn");
            page1End = true;
        }
        //区块三
        if ($(document).scrollTop() >= $(".column2").offset().top - 400 && !page2End) {
            $("#area2-1").removeClass('flipOutX').css("visibility","visible").addClass("flipInX");
            page2End = true;
        }
        //区块四
        if ($(document).scrollTop() >= $(".column3").offset().top - 400 && !page3End) {
            $("#area3-1").removeClass('slideOutLeft').css("visibility","visible").addClass("slideInLeft");
            $("#area3-2").removeClass('slideOutRight').css("visibility","visible").addClass("slideInRight");
            page3End = true;
        }
    }
    if (document.addEventListener) { document.addEventListener('DOMMouseScroll', scrollFunc, false); }
    window.onmousewheel = document.onmousewheel = scrollFunc;
    scrollFunc();

})(jQuery);
```



 




