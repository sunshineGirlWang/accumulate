http://zh.learnlayout.com/margin-auto.html

https://www.zhihu.com/question/24826065

一、display属性

1、块级元素：block

1.1、div是一个标准的块级元素。一个块级元素会新开始一行并且尽可能撑满容器。

1.2、其他块级元素包括p、form和HTML5中的新元素：header、footer、section等等。

2、行内元素：inline

  如span、a等。

3、none

3.1、一些特殊元素的默认display是它，比如script。

3.2、none与visibility属性不一样。

  区别：display:none的元素不会占据它本来应该显示的空间。但visibility:hidden还会占据空间。

4、inline-block

4.1、vertical-align属性会影响到inline-block元素，你可能会把它的值设置为top。

4.2、你需要设置每一列的宽度。

4.3、如果HTML源代码中元素之间有空格，那么列与列之间会产生空隙。

5、list-item

table

inline-block

flex

grid


二、margin:auto;

1、width
    #main{
    width: 600px;
    margin: 0 auto;
    }
  设置块级元素的width可以防止它从左到右撑满整个容器。
  然后你就可以设置左右外边距为auto来使其水平居中。
  元素会占据你所指定的宽度，然后剩余的宽度会一分为二为左右外边距。
  
  缺点：当浏览器窗口比元素的宽度还要窄时，浏览器会显示一个水平滚动条来容纳页面。

2、max-width

  #main{
  max-width: 600px;
  margin: 0 auto;
  }
    用max-width替代width可以使浏览器更好地处理小窗口的情况。
    这点在移动设备上显得尤为重要。（主流浏览器都适用的）

三、盒模型

 1、即使设置了元素的宽度，元素的边框和内边框会撑开元素。
 
2、box-sizing: border-box;

2.1、指定宽度和高度（最小/最大属性）确定元素边框box。

2.2、设置这个属性以后，设置的内边距和边框并不会再增加它的宽度。

2.3、支持IE8+的，要使用-webkit-和 -moz-。

四、position

1、static

  默认值，任意position:static;的元素不会被特殊的定位。
  一个static元素表示它不会被“positioned”，一个position属性被设置为其他值的元素表示它会被“positioned”。

2、relative    相对定位

2.1、relative表现的和static一样，除非添加了一些额外的属性。

2.2、在一个相对定位的元素上设置top、right、bottom和left属性会使其偏离其正常位置。
其他的元素的位置则不会受该元素的影响发生位置改变来弥补它偏离后剩下的空隙。

3、fixed    固定定位

3.1、fixed元素会相对于视窗来定位的，这意味着即便页面滚动，它还是会停留在相同的位置。

3.2、和relative一样，top、right、bottom和left属性都可用。

3.3、一个固定定位元素不会保留它原本在页面应有的空隙（脱离文档流）。

3.4、但是移动浏览器对flex的支持很差。

解决方案：待补充

4、absolute  绝对定位

4.1、与fixed表现类似，但不是相对于视窗而是相对于最近的“positioned”祖先元素。

4.2、如果绝对定位的元素没有“positioned“祖先元素，那么它是相对于文档的body元素，并且它会随着页面滚动而移动。

4.3、一个“positioned”元素是指position值不是static的元素。

五、float

1、clear属性用于控制浮动。可以使用left、right或both来清除向左、向右或同时清除向左向右的浮动。

2、清除浮动（clearfix hack）

  .clearfix{
  overflow:auto;
  //zoom:1;//如果要支持IE6，需要加上这行
  }
  
3、百分比宽度

六、媒体查询

1、响应式设计（Responsive Design）是一种让网站针对不同的浏览器和设备“呈现”不同显示效果的策略。
媒体查询就是其强大的工具。
  @media screen and (min-width:600px){  }
  @media screen and (max-width:599px){  }

2、使用 meta viewport之后可以让你的布局在移动浏览器上显示的更好。
  https://developer.mozilla.org/zh-CN/docs/Mobile/Viewport_meta_tag
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1"

七、column  可以实现文字的多列布局

column-count: 3;
column: 1em;

新标准，所以要加前缀。但不会被IE9及以下和Opera Mini支持。

八、flexbox布局

使用flexbox的居中布局

    .vertical-container{
    height: 300px;
    display: -webkit-flex;
    diisplay: flex;
    -webkit-align-items： center;
    align-items: center;
    -webkit-justify-content: center;
    justify-content: center;
    }








