### element-react的相关使用说明

#### 一、参考资料

#### 二、具体内容
***
**1.TimePicker**

（1）TimePicker的默认时间要设置成标准时间格式，其value值也是标准时间格式的。可以使用new Date()来设置。
     
    //此组件是用来选择时分秒的，所以年月日可以随意填写
    let defaultTimePicker = new Date(2018,1,1,0,0,0);

（2）可能需要将标准时间格式转换为接口需要的格式，比如'YYYY-MM-DD HH:mm:ss'。  
    
***


#### 三、组件的修改与扩充
有的组件并不能满足实际的需要，所以会在其内容上增加一下修改以及扩充。

***
**1. Upload增加支持“only-picture”(只展示图片)的listType。** 

    思路：增加this.props.listType名为"only-picture"的类型。

    修改点：

    （1）在Upload/Upload.js中，Upload.propTypes.listType增加一个"only-picture"的参数。

    （2）在Upload/UploadList.js中，在render方法中，

            <li
                className={this.classNames({
                'ishow-upload-list__item': true,
                [`is-${file.status}`]: true,
                'ishow-upload-only-picture': listType !== 'only-picture'?false:true
                })}
                key={file.uid}
            >
                {['picture-card', 'picture','only-picture'].includes(listType) &&
                isFinished(file.status) &&
                <img
                    className="ishow-upload-list__item-thumbnail"
                    src={file.url}
                    alt=""
                />}

                {listType !== 'only-picture'?
                <a
                    className="ishow-upload-list__item-name"
                    onClick={() => onPreview(file)}
                >
                    <i className="ishow-icon-document" />{file.name}
                </a>
                :''
                }
                
                <label
                    className="ishow-upload-list__item-status-label"
                >
                    <i
                    className={this.classNames({
                        'ishow-icon-upload-success': true,
                        'ishow-icon-circle-check': listType === 'text',
                        'ishow-icon-check': ['picture-card', 'picture','only-picture'].includes(
                            listType
                        )
                    })}
                    />

                </label>
                <i className="ishow-icon-close" onClick={() => onRemove(file)} />
                <View
                    className="ishow-upload-list__item-actions"
                    show={listType === 'picture-card' && isFinished(file.status)}
                >
                    <span>
                        <span
                            onClick={() => onPreview(file)}
                            className="ishow-upload-list__item-preview"
                        >
                            <i className="ishow-icon-view" />
                        </span>
                        <span
                            className="ishow-upload-list__item-delete"
                            onClick={() => onRemove(file)}
                        >
                            <i className="ishow-icon-delete2" />
                        </span>
                    </span>
                </View>
                {file.status === 'uploading' &&
                <Progress
                    strokeWidth={listType === 'picture-card' ? 6 : 2}
                    type={listType === 'picture-card' ? 'circle' : 'line'}
                    percentage={parseInt(file.percentage, 10)}
                    status={
                    isFinished(file.status) && file.showProgress ? 'success' : ''
                    }
                />}
            </li>

    （3）在Common/css/Upload.css中增加：

        .ishow-upload-only-picture{
            width:125px;
            display:inline-block;
            margin-right:10px;
        }
        .ishow-upload-only-picture .ishow-upload-list__item-thumbnail{
            width:100px;
            height:100px;
        }

***
**2. TimePicker的组件增加支持时分的选择（原本只支持时分秒的选择）。**
    
    思路：通过format的设置，改变isShowSeconds的值。

    修改点：

    （1）在DatePicker/TimePicker.js中，converSelectRange方法修改如下：

            const format = props.format=="HH:mm"?props.format: DEFAULT_FORMATS.timerange;

    （2）在DatePicker/panel/TimePanel.js中，render方法修改如下：

            const seconds = isShowSeconds?currentDate.getSeconds():null;

    （3）在DatePicker/basic/TimeSpinner.js，constructor方法修改如下：
            
            //如果isShowSeconds为false，seconds设置为null
            this.state.seconds = props.isShowSeconds?this.state.seconds:null;

***
**3. Checkbox的文本说明，增加title。**

    思路：增加this.props.showTitle的属性，如果showTitle为true，则展示title。

    修改点：

        （1）在Checkbox/CheckBox.js中，render方法中修改如下：
    
            let title = this.props.showTitle?(this.props.children || this.state.label):'';

            <span className="ishow-checkbox__label" title={title}>
                {this.props.children || this.state.label}
            </span>

***
**4. Table的展开行可以自定义展开title与内容。**

    思路：在column中增加expandText和expandTitle两个值。

    修改点：

        (1)在Table/TableBody.js中，renderCell方法中修改如下：

            const { type, selectable,expandText,expandTitle } = column;
            if (type === 'expand') {
                return (
                    <div
                        className={this.classNames('ishow-table__expand-icon ', {
                            'ishow-table__expand-icon--expanded': this.context.store.isRowExpanding(row, rowKey),
                    })}
                        onClick={this.handleExpandClick.bind(this, row, rowKey)}
                    >

                    {
                        expandText?
                            <div><span>{expandTitle}</span><i className="ishow-icon ishow-icon-arrow-right" /></div>
                        :<span>收缩</span>
                    }               
                    </div>
                )
            }


***
**5. Table中正常使用Select** 

    思路：在Select中增加selectUnderTable={true}，
         将.ishow-select-dropdown设置为position:fixed，再计算其正确的top值。

    修改点：

    （1）在Select/Select.js中，

        A.增加全局样式：

            .ishow-select-dropdown.ishow-select-undertable {
                position: fixed !important;
            }

        B.在onEnter() 方法的new Popper中增加一个option：

            selectUnderTable: this.props.selectUnderTable   

        C.在渲染.ishow-select-dropdown时，根据标识增加一个className：

            <div ref="popper" className={this.classNames('ishow-select-dropdown', {
                'is-multiple': multiple
            },this.props.selectUnderTable?'ishow-select-undertable':'')} style={{
              minWidth: inputWidth,
            }}>

    （2）在Common/popper.js中，

        A.Popper.prototype.update方法中，增加一行传值：

            data.selectUnderTable = this._options.selectUnderTable;

        B.Popper.prototype.modifiers.applyStyle方法中，根据窗口滚动条高度计算top值：

            if(data.selectUnderTable){//表格内部的Select框    
                styles.top = top-this.getScrollTop();
            }else{
                styles.top = top;
            }

        C.增加一个获取窗口滚动条高度的方法：

            Popper.prototype.getScrollTop = function() {
                var scrollTop = 0;
                if (document.documentElement && document.documentElement.scrollTop) {
                scrollTop = document.documentElement.scrollTop;
                } else if (document.body) {
                scrollTop = document.body.scrollTop;
                }
                return scrollTop;
            } 

***
**6.Table中的一列复选框，不需要展示title**

    思路：根据column中type值，区分是否显示title

    修改点：

    （1）在Table/TableBody.js，renderCell方法中：
       
         renderCell(row, column, index, rowKey){
            const { type, selectable,expandText,notShowTitle } = column;//引入notShowTitle

            ...
        }

    （2）在Table/TableBody.js，render方法中：
        <div className="cell" 
            title={!(column.type == "selection" && column.notShowTitle) && this.renderTitle(this.renderCell(row, column, rowIndex, rowKey))}
        >
            {this.renderCell(row, column, rowIndex, rowKey)}
        </div>

***
