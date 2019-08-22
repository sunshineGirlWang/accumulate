### 一、flex布局的链接：

    https://segmentfault.com/a/1190000008823763

### 二、基本知识点
1、container的7个属性：

    display、flex-direction、flex-wrap、flex-flow、justify-content、align-items、align-content

    (1)display: flex|inline-flex;
        用来定义父元素是一个 flex布局容器。如果设置为flex则父元素为块状元素，设置为inline-flex父元素呈现为行内元素。

    (2)flex-direction: row | row-reverse | column | column-reverse;  
        定义flex布局的主轴方向。flex布局是单方向布局，子元素主要沿着水平行或者垂直列布局。

        A.row: 行方向，flex-direction的默认值，
            在ltr(left to right， 从左到右)排版方式下从左到右排列，
            在rtl(right to left， 从右到左)排版方式下从右到左排列。
        B.row-reverse: 行反方向，在ltr中从右向左，在rtl中从左到右。
        C.column: 列方向，与row相似，只是从上到下。
        D.column-reverse: 列反方向，与row-reverse相似，只是从下到上。

    (3)flex-wrap: nowrap | wrap | wrap-reverse;
        默认情况下，flex布局中父元素会把子元素尽可能地排在同一行，通过设置flex-wrap来决定是否允许子元素这行排列。

        A.nowrap: 不折行，默认值，所有的子元素会排在一行。
        B.wrap: 折行，子元素会从上到下根据需求折成多行。
        C.wrap-reverse: 从下向上折行，子元素会从下岛上根据需求折成多行。

    (4)flex-flow: <‘flex-direction’> || <‘flex-wrap’>;
       flex-direction和flex-wrap属性的缩写形式。默认值是row,nowrap。

    (5)justify-content: flex-start | flex-end | center | space-between | space-around;
        定义了子元素沿主轴方向的对齐方式，用来当子元素大小最大的时候，分配主轴上的剩余空间。
        也可以当子元素超出主轴的时候用来控制子元素的对齐方式。

        A.flex-start: 默认值，朝主轴开始的方向对齐。
        B.flex-end: 朝主轴结束的方向对齐。
        C.center: 沿主轴方向居中。
        D.space-between: 沿主轴两端对齐，第一个子元素在主轴起点，最后一个子元素在主轴终点。
        E.space-around: 沿主轴子元素之间均匀分布。
            要注意的是子元素看起来间隙是不均匀的，第一个子元素和最后一个子元素离父元素的边缘有一个单位的间隙，
            但两个子元素之间有两个单位的间隙，因为每个子元素的两侧都有一个单位的间隙。

    (6)align-items: flex-start | flex-end | center | baseline | stretch;
        定义了子元素在交叉轴方向的对齐方向，这是在每个子元素仍然在其原来所在行的基础上所说的。
        可以看作是交叉轴上的justify-content属性;

        A.flex-start: 按照交叉轴的起点对齐。
        B.flex-end: 按照交叉轴的终点对齐。
        C.center: 沿交叉轴方向居中。
        D.baseline: 按照项目的第一行文字的基线对齐。
        E.stretch: 默认值，在满足子项目所设置的min-height、max-height、height的情况下拉伸子元素使之填充整个父元素。

    (7)align-content: flex-start | flex-end | center | space-between | space-around | stretch;
        当父元素所包含的行在交叉轴方向有空余部分时如何分配空间。
        与justify-content在主轴上如何对单个子元素对齐很相似。

        注意：当只有一行的时候，该属性并不起作用。
    

    ~~注意:align-items和align-content的区别，前者是指在单行内的子元素对齐方式，后者是指多行之间的对齐方式。


2、item的6个属性：

    order、flex-grow、flex-shrink、flex-basis、flex、align-self

    (1)order: <integer>;
        默认情况下，子元素按照代码书写的先后顺序布局，但order属性可以更改子元素出现的顺序。

        注意：order的默认值为0;子元素的order值越小，布局越排在前面，参考例图理解。

    (2) flex-grow: <number>; 
        规定在空间允许的情况下，子元素如何按照比例分配可用剩余空间。
        如果所有的子元素的属性都设定为1，则父元素中的剩余空间会等分给所有子元素。
        如果其中某个子元素的flex-grow设定为2，则在分配剩余空间时该子元素将获得其他元素二倍的空间（至少会尽力获得）。

        注意：
        A.flex-grow不接受负值。
        B. 默认值为0，意味着即使有剩余空间，各子元素也不会放大。

    (3)flex-shrink: <number>;
        注意：
        A.flex-shrink不接受负值。
        B.默认值为1， 当所有子元素都为默认值时，则空间不足时子元素会同比例缩小。
        如果其中某个子元素的flex-shrink值为0，则空间不足时该子元素并不会缩小。
        如果其中某个子元素的flex-shrink值为2时，则空间不足时该子元素会以二倍速度缩小。

    (4)flex-basis: <length> | auto; 
        定义了在计算剩余空间之前子元素默认的大小。

    (5)flex
        flex-grow、flex-shrink、flex-basis三个属性的缩写。
        其中第二个和第三个参数(flex-grow,flex-basis)是可选的。
        默认值为0 1 auto。

    (6)align-self: auto | flex-start | flex-end | center | baseline | stretch;
    通过设置某个子元素的align-self属性，可以覆盖align-items所设置的对齐方式。
    属性值与align-items中的意义相同。

    注意：float,clear和vertical-align对flex子元素没有任何影响。