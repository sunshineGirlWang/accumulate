### antd与element-react组件的异同点

#### 一、Table组件
1. antd

    (1)使用dataSource存放表格的数据。
    
    (2)columns中render(text, record, index) {}:参数分别为当前行的值，当前行数据，行索引.
   
    (3)columns中dataIndex：列数据在数据项中对应的 key，支持 a.b.c 的嵌套写法。
2. element-react

    (1)使用data存放表格的数据。

    (2)columns中render(data):data代表data[index]的值。

    (3)