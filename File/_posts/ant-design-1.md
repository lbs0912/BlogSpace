---
title:  Ant Design Notes-1 IE9兼容踩坑
date: 2017-09-03 22:35:48
categories: Ant Design
tags: [Ant Design,IE9兼容]
keywords: Ant Design,IE9兼容
---




* [Ant Design](https://ant.design/index-cn)
* 本文主要对京东实习期间，对Ant Design在IE9下兼容性情况的处理进行记录。

<!--more-->

## Table组件中rowKey指定

> 按照 `React` 的规范，所有的组件数组必须绑定 `key`。在 `Table` 中，`dataSource` 和 `columns` 里的数据值都需要指定 `key` 值。对于 `dataSource` 默认将每列数据的 `key` 属性作为唯一的标识。
> 
> 如果你的数据没有这个属性，务必使用 `rowKey` 来指定数据列的主键。若没有指定，控制台会出现以下的提示，表格组件也会出现各类奇怪的错误。--- [Ant Design](https://ant.design/components/table-cn/)

```cpp
// 比如你的数据主键是 uid
return <Table rowKey="uid" />;
// or
return <Table rowKey={record => record.uid} />;
```

参考Ant Design官网对Table组件中`rowKey`的描述可知，**当数据没有key值时，需要使用`rowKey` 来指定数据列的主键。**

在实际开发过程中，遇到如下BUG：Chrome下Table组件的数据正常显示，但IE下Table组件的数据不显示。

![ant-design-table-1.png](http://ol3kbaay9.bkt.clouddn.com/ant-design-table-1.png)

![ant-design-table-2.png](http://ol3kbaay9.bkt.clouddn.com/ant-design-table-2.png)

该BUG的原因如上所述。解决办法是，为Table组件添加`rowKey`值。

```javascript
<Table dataSource={dataSource} columns={columns} rowKey={record => record.uid}/>
```

需要注意的是，Table中数据的`rowKey`值必须唯一，如果数据源没有唯一的标识值，可以使用时间戳或者随机数作为`rowKey`值。

```javascript
//使用时间戳作为rowKey
<Table dataSource={dataSource} columns={columns} rowKey={new Date().valueOf()}/>

//使用随机数作为rowKey
<Table dataSource={dataSource} columns={columns} rowKey={Math.random()*100}/>
```

## Tabs组件中type设定

在开发过程中遇到如下BUG：如下图所示，创建的4个Tab页面，每次点击对应的Tab标题时候，对应内容通过`transition`动画，在X轴方向上平移到视图区进行显示。

![ant-design-tabs-1.JPG](http://ol3kbaay9.bkt.clouddn.com/ant-design-tabs-1.JPG)

Chrome浏览器下，该功能可以正常运行。但是在IE 9下，切换Tab页面时，对应的Tab内容无法显示，页面只显示初始化时候的Tab内容（即图中“正常采购单”对应的Tab内容）。

查询资料和[Can I use CSS](http://caniuse.com/#search=transition)后发现，CSS中`transition`属性在IE11以下不兼容。（CSS 3新特性）

![/ant-design-tabs-2.JPG](http://ol3kbaay9.bkt.clouddn.com/ant-design-tabs-2.JPG)


解决办法是，参考[Ant Design](https://ant.design/components/tabs-cn/)文档，对`Tabs`组件添加`type="card"`属性值。

```javascript
ReactDOM.render(
  <Tabs onChange={callback} type="card">
    <TabPane tab="Tab 1" key="1">Content of Tab Pane 1</TabPane>
    <TabPane tab="Tab 2" key="2">Content of Tab Pane 2</TabPane>
    <TabPane tab="Tab 3" key="3">Content of Tab Pane 3</TabPane>
  </Tabs>
, mountNode);
```