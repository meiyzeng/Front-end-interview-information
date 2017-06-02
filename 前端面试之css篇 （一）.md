1 . `CSS 属性是否区分大小写？`

    > 答：不区分。 HTML，CSS都对大小写不敏感，但为了更好的可读性和团队协作一般都小写，而在XHTML 中元素名称和属性是必须小写的。

***

2 . `行内(inline)元素 设置margin-top和margin-bottom 是否起作用？padding-top和padding-bottom是否会增加它的高度？`

    > 答：行内元素又分为替换元素（replaced element）和非替换元素（non-replaced element）。
    >**替换元素**:  是指用作为其他内容占位符的一个元素。如： img、input 等起作用;
    >**非替换元素**:是指内容包含在文档中的元素 如：span等不起作用；

***

3 . `设置p的font-size:10rem，当用户重置或拖曳浏览器窗口时，文本大小是否会也随着变化？`

    > 答：rem是以html根元素中font-size的大小为基准的相对度量单位，文本的大小不会随着窗口的大小改变而改变。

***

4 . `translate()方法能移动一个元素在z轴上的位置？`

    > 答：不能。translate()方法只能改变x轴，y轴上的位移。

***

5 . `only 选择器的作用是？ @media only screen and (max-width: 1024px) {margin: 0;}`

    > 答：停止旧版本浏览器解析选择器的其余部分。
    only 用来定某种特定的媒体类型，可以用来排除不支持媒体查询的浏览器。其实only很多时候是用来对那些不支持Media Query 但却支持Media Type 的设备隐藏样式表的。其主要有：支持媒体特性（Media Queries）的设备，正常调用样式，此时就当only 不存在；对于不支持媒体特性(Media Queries)但又支持媒体类型(Media Type)的设备，这样就会不读了样式，因为其先读only 而不是screen；另外不支持Media Qqueries 的浏览器，不论是否支持only，样式都不会被采用。

***

6 . `浏览器CSS匹配顺序`

    > 答：浏览器CSS匹配不是从左到右进行查找，而是从右到左进行查找。比如#divBox p span.red{color:red;}，浏览器的查找顺序如下：先查找html中所有class='red'的span元素，找到后，再查找其父辈元素中是否有p元素，再判断p的父元素中是否有id为divBox的div元素，如果都存在则匹配上。浏览器从右到左进行查找的好处是为了尽早过滤掉一些无关的样式规则和元素。

***

7 . `display:none 和visibilty:hidden的区别：`

    > 答：display:none和visibility:hidden都是把网页上某个元素隐藏起来的功能，但两者有所区别， visibility:hidden属性会使对象不可见，但该对象在网页所占的空间没有改变（看不见但摸得到），等于留出了一块空白区域，而display:none`属性会使这个对象彻底消失（看不见也摸不到）

***

8 . `请描述 BFC(Block Formatting Context) 及其如何工作。：`

    > 答：浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的BFC（块级格式上下文）

    > 会触发BFC的条件有：

    - float的值不为none。
    - overflow的值不为visible。
    - display的值为table-cell, table-caption, inline-block 中的任何一个。
    - position的值不为relative 和static。

    > 折叠的结果：

    - 两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
    - 两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
    - 两个外边距一正一负时，折叠结果是两者的相加的和。

***

9 . `谈谈样式的优先级`

    > 优先级逐级增加

    1. 0.元素(类型)选择器 （h1）  和 伪元素选择器 （:before）
    2. 1.类选择器（.demo）属性选择器（[type="radio"]）
    3. 2.ID选择器（#demo）
    4. 内联样式

    > 当在一个样式声明上使用 !important规则时，该样式声明会覆盖CSS中任何其他的声明。
    一些经验法则：

    + Never 永远不要在全站范围的 css 上使用 !important
    + Only 只在需要覆盖全站或外部 css（例如引用的 ExtJs 或者 YUI ）的特定页面中使用   !important
    + Never 永远不要在你的插件中使用 !important
    + Always 要优化考虑使用样式规则的优先级来解决问题而不是 !important

***

10 . `了解过FOUC吗？如何解决FOUC`

    > 答：Flash of Unstyled Content (FOUC) 文档样式短暂失效
只需要在文档的head元素中添加一个link元素或者添加script元素就可以防止FOUC的发生.

link元素解决方案

    <head>
        <title>My Page</title>
        <link rel=”stylesheet” type=”text/css” media=”print” href=”print.css” mce_href=”print.css”>
        <style type=”text/css” media=”screen”>@import “style.css”;</style>
    </head>


script元素解决方案

    <head>
        <title>My Page</title>
        <script type=”text/javascript”> </script>
        <style type=”text/css” media=”screen”>@import “style.css”;</style>
    </head>

***