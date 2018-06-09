### element-react的相关使用说明

#### 一、参考资料

#### 二、具体内容
    
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

            this.state.seconds = props.isShowSeconds?this.state.seconds:null;//如果isShowSeconds为false，seconds设置为null

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

        (1)在Table/TableBody中，renderCell方法中修改如下：

            const { type, selectable,expandText,expandTitle } = column;
            if (type === 'expand') {
                return (
                    <div
                    className={this.classNames('ishow-table__expand-icon ', {
                        'ishow-table__expand-icon--expanded': this.context.store.isRowExpanding(row, rowKey),
                    })}
                    onClick={this.handleExpandClick.bind(this, row, rowKey)}
                    >
                    {expandText?<div><span>{expandTitle}</span><i className="ishow-icon ishow-icon-arrow-right" /></div>:<span>收缩</span>}
                        
                    </div>
                )
            }


***
**5.**