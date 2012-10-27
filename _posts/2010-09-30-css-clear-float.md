---
layout: post
title: CSS清除浮动
---

在文章开始前,建议看一下这篇文章:[All About Floats][],
其讲解了相当多关于CSS浮动的内容,包括浮动产生的问题以及怎样清除浮动.

## CSS清除浮动的方式

我认为清除浮动应该分为两部分:包含块外清除浮动与包含块内清除浮动.
包含块外当然就是指的 **clear: both;** 清除的浮动,
是用来避免块元素间的对齐问题;
而我们目前大家所说的CSS清除浮动一般都是指的包含块内清除浮动,这也是技术在不断发展,
布局技术在不断进步的必然结果, 详细的布局方面思考会在以后总结一下.
总归一下我使用过的包含块内部清除浮动方式:

-   包含块和其子块同时float
-   clear: both;
-   `:after`
-   overflow:hidden

### 包含块和其子块同时float

例如这个例子:


    <div style="float:left; width:100%; background:#000; color:#ddd">

      <div style="float:left; width:30%; height:100px; background:#F00"><p>Some content</p></div>

    </div>

可以看到,
ie6,7是由于width触发了hasLayout,而包含块浮动是针对其它浏览器起作用的.对于布局上来说,不是很方便,而且如果有很多子块的时候,每一个都得去控制浮动,
多么惨的事呢.

### clear: both;

    <div style="background:#000; color:#ddd" >

      <div style="float:left; width:30%; height:100px; background:#F00"><p>Some content</p></div>
        <div style="clear:both;"></div>
    </div>

使用这种方法,倒是不会出现什么大乱子,但是在html文本中必须使用额外的空标签去做清除的工作,对于追求标准的我们,这是不可容忍的.

### `:after`

    <style type="text/css">
    .clear:after{content: ".";display: block;height: 0;clear: both;visibility: hidden;}
    .clear{*zoom:1;}
    </style>
    <div style="background:#000; color:#ddd"  class="clear">

      <div style="float:left; width:30%; height:100px; background:#F00"><p>Some content</p></div>

    </div>

对于使用:after伪类选择符这个恐怕是现金为止认为最安全的方式了,各大css框架要么在布局上下功夫而不使用clearfix,要么就是使用的这种方式,
但各家清除的方式又存在细微差别. 例如,
[tjkdesign][]提出了在使用:after的同时,也要使用:before.[例子][]

### overflow:hidden

    <style type="text/css">
    body {background:#333;font:1em Arial, Helvetica, sans-serif;}
    h1 {color:#ececec;text-align:center;margin:1.5em 0 1em;}
    h2 {font-weight:normal;font-size:1.15em;padding-bottom:5px;border-bottom:1px solid #999;}
    p {padding-right:1em;color:#000;}
    pre {font-size:1em;color:#06f;margin:1em 0;}

    #wrapper {background:#ececec;overflow:hidden;zoom:1;padding:20px;border-bottom:1px solid #000;border-right:1px solid #000;border-top:1px solid #fff;border-left:1px solid #fff;}
    .css {float:right;width:50%;}
    .markup {float:left;width:50%;margin-right:-1px;}
    .box1,
    .box2,
    .box3,
    .box4 {background:#fff;position:absolute;padding:10px;border:1px solid #333;}
    .box1 {left:0;top:0;}
    .box2 {right:0;top:0;}
    .box3 {left:0;bottom:0;}
    .box4 {right:0;bottom:0;}
    </style>
    <h1>overflow:hidden and absolutely positioned elements</h1>
    <div id="wrapper">
    <div class="markup">
    <h2>Markup</h2>
    <pre>&lt;div id=&quot;wrapper&quot;&gt;

      &lt;div class=&quot;box1&quot;&gt;box 1&lt;/div&gt;
      &lt;div class=&quot;box2&quot;&gt;box 2&lt;/div&gt;
      &lt;div class=&quot;box3&quot;&gt;box 3&lt;/div&gt;

      &lt;div class=&quot;box4&quot;&gt;box 4&lt;/div&gt;
    &lt;/div&gt;</pre>
    <p>If this wrapper was <em>positioned</em> (any value other than static), then it would become the positioning block and the four nested boxes would position themselves in relation to its layout. They would also be clipped outside of the positioning block boundaries.</p>
    </div>
    <div class="css"> 
    <h2>CSS</h2>

    <pre>#wrapper {
      overflow:hidden;
      zoom:1;
    }
    .box1,
    .box2,
    .box3,
    .box4 {
      position:absolute;
      background:#fff;
    }
    .box1 {left:0;top:0;}
    .box2 {right:0;top:0;}
    .box3 {left:0;bottom:0;}
    .box4 {right:0;bottom:0;}</pre>
    </div>
          <div class="box1">box 1</div>
            <div class="box2">box 2</div>
            <div class="box3">box 3</div>
            <div class="box4">box 4</div>
    </div>

对于这个例子,我想是最好的解决了clearfix的烦琐和overflow:hidden的截断问题,因为overflow触发了Block
Formatting Contexts, 而zoom是给ie6和7用的.
而内部box具有了’position:absolute;’, 就不会受这个overflow的限制.
但是这会有一个问题:我测试了两台纯ie6环境,
都没有通过.其他浏览器都ok(包括ie8的兼容模式). 会触发**block formatting
contexts**的元素包括:

-   floats
-   absolutely positioned elements
-   inline-blocks
-   table-cells
-   table-captions
-   elements styled with “overflow” (any value other than “visible”)

所以使用 ‘display: inline-block;width: any explicit value;’
也是可以达到兼容性的. 欢迎交流指正.

## 参考阅读


-   [clearfix Reloaded + overflow:hidden Demystified][]
-   [Block Formatting Contexts][]
-   [All About Floats][]

  [clearfix Reloaded + overflow:hidden Demystified]: http://www.yuiblog.com/blog/2010/09/27/clearfix-reloaded-overflowhidden-demystified/
  [Block Formatting Contexts]: http://www.tjkdesign.com/articles/block-formatting-contexts_and_hasLayout.asp
  [All About Floats]: http://css-tricks.com/all-about-floats/
  [tjkdesign]: http://www.tjkdesign.com/articles/clearfix_block-formatting-context_and_hasLayout.asp
  [例子]: http://www.tjkdesign.com/lab/clearfix/new-clearfix.html