### element-react的相关使用说明

#### 一、参考资料

#### 二、具体内容

#### 三、组件的修改与扩充
有的组件并不能满足实际的需要，所以会在其内容上增加一下修改以及扩充。

1. Upload增加支持“only-picture”(只展示图片)的listType。 
    
2. TimePicker的组件增加支持时分的选择（原本只支持时分秒的选择）。

3. Checkbox的文本说明，增加title。

    增加this.props.showTitle的属性，如果showTitle为true，则展示title。

    组件内部，增加一行：
    
            title={this.props.showTitle?txt:''}

4. 