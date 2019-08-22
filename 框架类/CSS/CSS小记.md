### CSS小记
1. 如果想让一个原本不能编辑的标签（如div、p、span标签）可编辑，加入contenteditable="true"。
    A. 这是H5新增加的属性。

    B.<p contenteditable="true">这是一段可编辑的段落。请试着编辑该文本。</p>

2. 两个行内元素之间会有一个小小的空隙，需要加font-size：0消除空隙。

3. 用伪类来消除浮动：
    .clearfix:after{
        content: ""; 
        display: block; 
        height: 0; 
        clear: both; 
        visibility: hidden;  
    }

    .clearfix {
        zoom: 1; 
    }

4. 内容超出显示省略号：
    overflow: hidden;
    text-overflow:ellipsis;
    white-space: nowrap;

5. 